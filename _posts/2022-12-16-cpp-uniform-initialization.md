---
title: C++ Uniform Initialization - Benefits & Pitfalls
author: Ian Y.E. Pan
date: 2022-12-15 14:44:00 -0500
categories: [Programming]
tags: [programming, tips]
---

> Scott Meyer's book, "Effective Modern C++", has a nice section
> talking about uniform initialization, specifically, how to
> "distinguish between `()` and `{}` when creating objects". This blog
> post summarizes some important tips that the author pointed out,
> along with some advice according to my experience and preferences.

## Understanding uniform initialization in C++

Uniform initialization, since C++11 (a.k.a. modern C++ era), is the
practice of using "brace initialization" `{}` to initialize a variable
or an object. To start with a simple example, we now have at least 5
different ways to initialize an integer variable `x` to `1`:

```cpp
int x = 1;   // Historically the most common way.
int x(1);    // Initializer in parentheses.
int x{1};    // Initializer in braces.
int x = {1}; // Using both braces and '=', treated the same as "int x{1};".
auto x{1};   // Type of x deduced to int (since C++17).
```

Why not just use the classic `int x = 1;` and call it a day? 

For one, we can now achieve a very consistent syntax as follows:

```cpp
class MyClass {
 public:
  MyClass() = default;
  MyClass(int _x) : x{_x} { ... }  // Uses {} in member initialization list

  auto return_vector() -> std::vector<int> {
    return {7, 8, 9};  // Uses {}
  }

 private:
  int x{};  // Uses {} for default initialization (zero for int)
};

int main() {
  MyClass obj{5};                                    // Uses {}
  int a{1};                                          // Uses {}
  std::vector<int> v{1, 2, 3};                       // Uses {}
  std::unordered_set<int> s{4, 5, 6};                // Uses {}
  std::unordered_map<int, int> m{ {1, 2}, {3, 4} };  // Uses {}
}
```

Another thing brace initialization solves is that it does not allow narrowing
conversions. So while this would work in legacy C-style C++:

```cpp
double d = 1.5;
int x = d; // x is 1 (double converts to int).
```

This would not work with `{}` (which is a good thing):

```cpp
int x{d}; // ERROR: cannot be narrowed.
```

Brace initialization forces us to type-cast values explicitly like so:

```cpp
int x{(int)d};              // Old C-style type-cast.
int x{int(d)};              // Old C++-style type-cast.
int x{static_cast<int>(d)}; // Best practice in modern C++.
```

Narrowing conversions can cause issues that are painful to debug. I
once spent over an hour tracking down why a calculation is
consistently off by a few decimals -- turns out it was caused by an
implicit `double` to `int` conversion deeply buried in my source code.

Another problem that uniform initialization solves is the pitfall of
"most vexing parse" in C++, which is how the compiler resolves
syntactic ambiguity in certain cases. The definition of "most vexing
parse" goes like this: *When C++ can't distinguish between "object
creation" and "function" declaration", the compiler defaults to
interpret the statement as a "function declaration".* In other words,
if a line can be interpreted as a function declaration, the compiler
will interpret it as a function declaration. Consider this commonly
used vector definition:

```cpp
std::vector<int> v(5, 0); // Initializes v to five zeros: {0, 0, 0, 0, 0}.
```

Simple as it is, this will not work if we were to do this as a private
member initialization!

```cpp
class MyClass {
 public:
  MyClass() { ... }

 private:
  std::vector<int> v(5, 0); // ERROR
};
```

This is because C++ will misinterpret this as a function declaration,
and even complain about missing parameter declarators. There are two
ways to solve this elegantly, apart from verbosely typing out `std::vector v{0,
0, 0, 0, 0};`. The first way is to move the initialization to the constructor:

```cpp
class MyClass {
 public:
  MyClass() : v(5, 0) { ... }

 private:
  std::vector<int> v;
};
```

The second way is the use "copy initialization", like so:

```cpp
class MyClass {
 public:
  MyClass() { ... }

 private:
  std::vector<int> v = std::vector<int>(5, 0);
};
```

Since C++17, copy initialization like the one above is guaranteed to
be optimized (no copy overhead). Even before C++17, the optimization
is likely to happen, too.

## Common Pitfalls of uniform initialization (brace initialization)

The first thing to look out for is when a variable declared with
`auto` uses brace initialization, its type could be deduced to
`std::initializer_list` (when you're combining it with the equal sign,
or if it has multiple elements). This is probably not what we intend,
so we should generally avoid declaring an `auto` variable with an
initializer list. In certain cases, it even results in an
error. Here's a comprehensive example:

```cpp
auto x{1};       // OK: x is of type int (since C++17)
auto x = {1};    // x is of type std::initializer_list<int>

auto x{1, 2};    // ERROR: Initializer for variable 'x' with type 'auto' contains multiple expressions.
auto x = {1, 2}; // x is of type std::initializer_list<int>
```

The second problem especially confuses C++ beginners when they just
started out using `std::vectors`. Notice the difference between these
two statements:

```cpp
std::vector<int> v(5, 0); // Vector v holds 5 zeros -> {0, 0, 0, 0, 0}
std::vector<int> v{5, 0}; // Vector v holds a 5 and a 0 -> {5, 0}
```

As the author of "Modern Effective C++" stated, this is now viewed as
a flawed design in `std::vector`. We should learn from this mistake
and design our constructors such that "the overload called won't be
affected whether the client uses parentheses or braces."

The third issue, and perhaps the most problematic one, is when
there's an overloading constructor that declares its parameter of type
`std::initializer_list`. Calling a constructor with the uniform
initialization syntax will "very strongly" prefer using this
overload. Here's an example to illustrate this point:

```cpp
class MyClass {
 public:
  MyClass(int x, double y) { ... }
  MyClass(std::initializer_list<bool> z) { ... }
};

int main() {
  MyClass obj{5, 1.0}; // ERROR (think, why?)
};
```

We might expect `MyClass obj{5, 1.0};` to call the first constructor
(the one with an `int` and a `double` as parameters), but since there's a
constructor overload that has `std::initializer_list` as parameter,
that constructor will be "very strongly preferred". In this case, C++
will even throw an error because it detects narrowing conversions from
`int` and `double` to `bool`. Imagine if there's no narrowing conversion
involved (for instance, the second constructor takes in
`std::initializer_list<double>`, the code will silently execute using
the second constructor (with `int 5` converted to `double 5.0` in the
initializer list), while the programmer thought it was using the first
constructor.

The story doesn't end here. There is one exception to this "strongly
prefer `std::initializer_list` constructors" rule. If we instantiate
an object with empty braces, and both default constructor and
`std::initializer_list` overload constructor exist, C++ will use the
default constructor. The following example showcases this exception:

```cpp
class MyClass {
 public:
  MyClass() { ... }
  MyClass(std::initializer_list<int> z) { ... }
};

int main() {
  MyClass obj{}; // Calls the first constructor.
};
```

If we mean to call the second constructor (with an empty initializer
list), we must do it either of the following ways:

```cpp
MyClass obj( {} ); // Calls second constructor with empty initializer list.
MyClass obj{ {} }; // Calls second constructor with empty initializer list.
```

## Conclusion

No matter which "default delimiter" (parentheses or braces) you
choose, it's always good to understand when and why we are using
one. Because, as this blog pointed out, sometimes we must use one or
the other. Personally I prefer to use uniform (brace) initialization
wherever possible, but also acknowledge certain constructors (like
`std::vector`) has an overload that takes a size and initial value and
we must use parentheses if we want to call that
constructor. Similarly, for the C++ programmers who prefer to use
parentheses wherever possible, it is important to know that there are
scenarios where parentheses won't work (for instance, in the case of
"most vexing parse").

I highly recommend reading this section in Scott Meyer's "Effective
Modern C++" book (the beginning of Chapter 3: Moving to Modern
C++). On top of that, I'd also recommend reading [this blog post by
Herb Sutter](https://herbsutter.com/2013/05/09/gotw-1-solution/) on
`()` vs. `{}` initialization.

That's all for today, thanks for stopping by.

## References:
- [https://www.oreilly.com/library/view/effective-modern-c/9781491908419/](https://www.oreilly.com/library/view/effective-modern-c/9781491908419/)
- [https://www.fluentcpp.com/2018/01/30/most-vexing-parse/](https://www.fluentcpp.com/2018/01/30/most-vexing-parse/)
- [https://en.wikipedia.org/wiki/Most_vexing_parse](https://en.wikipedia.org/wiki/Most_vexing_parse)
- [https://abseil.io/tips/88](https://abseil.io/tips/88)
- [https://herbsutter.com/2013/05/09/gotw-1-solution/](https://herbsutter.com/2013/05/09/gotw-1-solution/)
