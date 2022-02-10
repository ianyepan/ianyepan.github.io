---
title: A Tour of C++ - Reading Notes (Part 1/2)
author: Ian Y.E. Pan
date: 2022-01-31 20:44:00 -0500
categories: [Programming]
tags: [programming, tips]
---

The following are some modern C++ features that I found interesting or unfamiliar at the time of reading "[A Tour of C++](https://www.google.com/books/edition/_/UGtRtAEACAAJ?hl=en&sa=X&ved=2ahUKEwjRt8jLuN31AhWhZjUKHd2uC2sQre8FegQIFBAG)" by Professor [Bjarne Stroustrup](https://en.wikipedia.org/wiki/Bjarne_Stroustrup), whose C++ course at Columbia University I am currently enrolled in. 

![Bjarne](/images/bjarne.jpg){: width="70%"}
_Bjarne Stroustrup, the creator of C++, and my professor at Columbia University_

This post (Part 1) consists of my reading notes for chapter 1 through 7, and serves as personal bullet points for me to look back on. Hopefully, by reading the points you'll find something useful as well.

1. (Ch.1 pp.10) `constexpr` means “to be evaluated at compile time”, which can boost performance. When using a `constexpr` function (which should not have any side-effects and not modify non-local variables) with non-constant parameters passed in, the result will not be a constant expression. E.g.

   ```cpp
   constexpr double half(double x) {return x / 2};
   int num = 4.0;
   constexpr auto numHalf = half(num); // WRONG! "num" is non-constant, 
                                       // result will not be constexpr!
   ```
2. C++17 If statement with initializer (Ch.1 pp.15). We can initialize a variable **and** use it for a condition all in an “if-statement”. This helps keep variables’ scopes tighter. E.g.

   ```cpp
   if (auto a = arr[0]; a != 's') { /*...*/ }
   ```

   If we’re testing the variable against zero, we can even omit the condition part. E.g.

   ```cpp
   if (auto sz = vec.size()) { /* loop entered if sz != 0 */}
   ```

3. (Ch.1 pp.17) "Assignment to reference doesn't change what the reference refers to (unlike pointers) but assigns to the referenced object". E.g.
   ```cpp
   int x = 2, y = 3;
   int &r1 = x;
   int &r2 = y;
   r1 = r2; // x becomes 3. r1 still points to x.
   ```
4. No uninitialized reference is allowed (Ch.1 p.18).
5. Unions (Ch.2 p.25) can be used when only one of the multiple candidate variables can be used at any given time.
   - That said, STL's `variant` (Ch.2 p.26) since C++17 can replace most use-cases of unions. It is a type-safe `union`.
   - We can use `try-catch` or `std::holds_alternative` in conjunction with `std::get<type>` to obtain the desired value.
6. Plain `enum` v.s. `enum class`. The latter is preferred (Ch.2):
   - Enumerator values of plain `enums` are implicitly converted (e.g. to `int`). Also, the enumerator names are "exported to the surrounding scope", potentially causing name clashes.
   - In `enum class` (preferred over `enum` due to its type safety), the same value names can be used in different `enum class`. E.g.
     ```cpp
     enum class Color1 { red, green, blue };
     enum class Color2 { red, yellow, black };
     ```
      One can distinguish between `Color1::red` and `Color2::red`.
   - However, in plain enums, this will not compile:
     ```cpp
     enum Color1 { red, green, blue };
     enum Color2 { red, yellow, black }; // ERROR! Redefinition of enumerator 'red'.
     ```
7. Separate Compilation (Ch.3 p.30): In the example of `user.cpp`, `Vector.h`, and `Vector.cpp`, `Vector.h` is the "interface" with declarations of the classes that `user.cpp` would use. The implementation details are specified in `Vector.cpp`. Both `.cpp` files "include" the `.h` file, and the `.cpp` files can be compiled separately.
8. Declarations and macros from an included header file, might affect the meaning of the later included header files (Ch.3 p.31): a significant cause of bugs!
9. (Ch.3 p.34) Differences between headers (old-school `#include`) and modules (`export/import`):
   - Modules are compiled once. Headers are compiled in each "translation unit" where it is used.
   - Avoid problem mentioned in previous bullet point (changing include order may change meaning).
   - Imports are not transitive. (i.e. X imports Y, and Z imports X. Z does not automatically have Y imported.)
10. "Namespaces are primarily used to organize larger program components, such as libraries." (Ch.3 p.35)
11. A function declared as `noexcept` should "never" throw an exception. Otherwise, `std::terminate()` is called to terminate the program (Ch.3 p.37).
12. Avoid naked "new" and "delete". Instead, keep the allocation and deallocation "buried inside the implementation of well-behaved abstractions", i.e. constructors and destructors. (Ch.4, p.52)
13. (Ch.4 p.54) "Pure virtual" functions are declared by assigning a zero. A derived class "must" define the function. E.g.
```cpp
class MyClass {
 public:
  virtual int size() = 0;
}
```
   Here, `MyClass` is an "abstract class" because it has a "pure virtual" function. We cannot create objects of an abstract class. Therefore, abstract classes usually don't have "constructors" either.
14. In inheritance, "objects are constructed 'base-class-first' by constructors and destroyed 'derived-class-first' by destructors." (Ch.4 p.61)
15. We can use `dynamic_cast` to achieve "is instance of" operations. (Ch.4 p.62)
```cpp
template<typename Base, typename T>
inline bool instanceof(T *ptr) {
  return dynamic_cast<Base*>(ptr) != nullptr;
}
```
16. "Use `unique_ptr` or `shared_ptr` to avoid forgetting to delete objects created using `new`." (Ch.4 p.64)
17. "Use `explicit` for constructors that take a single argument unless there is a good reason not to." (Ch.5 p.68). Example:
```cpp
class Vector {
 public:
  explicit Vector(int sz); // avoid implicit conversion from int to Vector
  /* ... */
}
Vector vec1(7);  // OK
Vector vec2 = 7; // NOT OK
```
18. `std::move()` is more like a cast. It doesn't actually move anything. (Ch.5 p.72)
19. Range-for loops use `.begin()` and `.end()` implicitly. (Ch.5 p.75)
20. (Ch.6 p.83) In C++17, `std::pair`'s template arguments (types) can be deduced:
```cpp
std::pair<int, double> p1 = {1, 3.41}; // pre C++17
std::pair              p2 = {1, 3.41}; // since C++17
```

![A Tour of Cpp](/images/cpp-book.jpg){: width="35%"}
_It's a pretty good read._


Read Part 2 [here](../cpp-notes-pt2).
