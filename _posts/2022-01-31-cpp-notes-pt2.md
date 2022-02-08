---
title: A Tour of C++ - Reading Notes (Part 2)
author: Ian Y.E. Pan
date: 2022-02-07 20:44:00 -0500
categories: [Programming]
tags: [programming, tips]
---

For Part 1, visit [this previous post](../cpp-notes-pt1).

The selected following are some modern C++ features that I found interesting or unfamiliar at the time of reading "[A Tour of C++](https://www.google.com/books/edition/_/UGtRtAEACAAJ?hl=en&sa=X&ved=2ahUKEwjRt8jLuN31AhWhZjUKHd2uC2sQre8FegQIFBAG)" by Professor [Bjarne Stroustrup](https://en.wikipedia.org/wiki/Bjarne_Stroustrup), whose C++ course at Columbia University I am currently enrolled in. 

![Bjarne](/images/bjarne1.jpg){: width="70%"}
_Bjarne Stroustrup, the creator of C++, and my professor at Columbia University_

This post (Part 2) consists of my reading notes for chapter 8 through 14. Hopefully, by reading the points you'll find something useful as well.


1. For each header file from C's standard library, there's a version "with its name prefixed by `c` and without `.h`". This version uses `std::` namespace (Ch.8 p.110).
   ```cpp
   // C style
   #include <stdlib.h>

   // C++ style (std::)
   #include <cstdlib> 
   ```
2. `std::string::replace()`'s new replacement string doesn't need to be the same length as the replaced target substring. (Ch.9 p.112)
3. String literals are `const char*` by default. If we want type `std::string`, we need the `s` suffix (Ch.9 p.113)
   ```cpp
   /*
    * The "s" suffix needs one of the following namespace to be included:
    *   - using namespace std::literals 
    *   - using namespace std::string_literals 
    *   - using namespace std::literals::string_literal
    */
   auto s1 = "Ian";  // const char* (C-style)
   auto s2 = "Pan"s; // std::string
   ```
4. "Short string optimization": short string values are kept in the string object (special optimization), and only longer strings are placed on free store (dynamically allocated memory on the heap) as the usual case. (Ch.9 p.113)
5. `string_view` is a read-only view of its characters (Ch.9 115).
6. "Raw string literals" starting with `R"(` and ending with `)"` allows backslashes to be used directly in strings. E.g. `"\\w{2}\\d{4}"` can become  `R"(\w{2}\d{4})"` (Ch.9 p.116).
7. (Ch.10 p.125) In output streams, the "result of an output expression can be used for further output". Therefore, we can chain multiple "put-to" (`<<`) statements like so:
   ```cpp
   std::cout << "Hi, ";
   std::cout << "my name is ";
   std::cout << "Ian.";

   // equivalent to

   std::cout << "Hi, " << "my name is " << "Ian.";
   ```
8. (Ch.10 p.126) The same can be said for input streams, chaining multiple "get-from" (`>>`) statements:
   ```cpp
   int num1, num2;
   double num3;
   std::cin >> num1;
   std::cin >> num2;
   std::cin >> num3;

   // equivalent to

   std::cin >> num1 >> num2 >> num3;
   ```
   The read of an integer will stop when a character is not a digit. Also, `>>` will skip initial whitespaces by default.
9. We can use `istream1 >> char1` as a condition, meaning "Did we successfully read a character from istream1 into char1?" (Ch.10 p.128)
10. `istream >> ch` skips whitespace by default, but `istream.get(ch)` does not (Ch.10 p.128).
11. Formatting with manipulators (placed between the output stream and the target to output) (Ch.10 p.129):
```cpp
std::cout << std::hex << 1234; // "4d2"
```
   Other manipulators include `std::oct`, `std::scientific`, `std::hexfloat`, etc.
12. (Ch. 10 p.130) `std::cout.precision()` or `std::setprecision()` can round floating numbers by a length limit. Note that it limits the length "in total", not just after the decimal point:
```cpp
std::cout.precision(7);
std::cout << 1234.567890; // 1234.568 (length 7 in total)
```
   However, if the original number was an integer, "precision" has no effect:
```cpp
std::cout.precision(4);
std::cout << 123456789; // 123456789 (not truncated/rounded at all)
```
   If we want to restore the default precision, we can store it before modifying it:
```cpp
auto x = std::cout.precision();
std::cout.precision(4);
// later restore
std::cout.precision(x);
```
13. (Ch.10 p.132) C-style I/O (printf, scanf) has better I/O performance. If we don't use C-style I/O but care about I/O performance, we can call:
```cpp
std::ios_base::sync_with_stdio(false);
```
14. The file system library `<filesystem>` allows us to express file system paths and navigate through a file system, examining file types and their associated permissions. (Ch.10 p.132)
15. `std::vector`'s subscript operator does not check range -- it'll give some random value rather than throwing an error! (Ch.11 p.141) This is because checking the range implies an extra 10%-order cost:
```cpp
std::vector<int> v{1,2,3,4};
std::cout << v[100]; // 0. No error thrown.
```
   - Use `.at()` to check range and throw out-of-range error:
```cpp
std::cout << v.at(100);
// terminate called after throwing an instance of 'std::out_of_range'
```
16. `std::list` are doubly-linked. Can be used over vectors if insertions and deletions are frequent (Ch.11 p.142).
   - There's also a singly-linked list in STL, called `std::forward_list` (Ch.11 p.143). This can save space, since each node will only have 1 pointer instead of 2.
17. Every STL container provides the functions `begin()` and `end()` (Ch.11 p.143).
18. `std::equal_range()` returns a pair that contains the values of `std::lower_bound()` and `std::upper_bound()` (Ch.11 p.143).
19. `std::map` (a.k.a. associative array, dictionary) is a balanced binary search tree, usually implemented by a red-black tree (Ch.11 p.144).
   - Use `find()` instead of `[]` to avoid inserting default values with keys that don't exist.
   - The cost of map lookup is O(log n).
20. Emplace operations, "such as `emplace_back()`, takes arguments for an element's constructor and builds the object in a newly allocated space in the container rather than copying an object into the container" (Ch.11 p.147):
```cpp
vector<pair<int, string>> v;
v.push_back(make_pair(1, "hello"));
v.emplace_back(2, "hi");
   ```
21. (Ch.11 p.148) We can often create good hash functions by combining standard hash functions for elements using the exclusive-or operator (`^`). 
22. `std::back_inserter()` returns an iterator (`std::back_insert_iterator`) that can be used to append values to the back of a container (Ch.12 p.150). With this function, we can avoid using `realloc()`, which is more C-style and error-prone.
23. Iterators can be great middlemen. STL algorithms can operate on a container's data through iterators without knowing anything about the container (Ch.12 p.153).
24. No STL algorithm adds or subtracts elements of a container (Ch.12 p.157). For that, we need container methods (`push_back()`, `erase()`) or functions that knows about the container, like `back_inserter()`.
25. Many STL algorithms, such as find, count, replace, sort, copy, have parallel versions, invoked by passing an extra argument:
```cpp
sort(begin(vec), end(vec));                      // default, sequential
sort(std::execution::seq, begin(vec), end(vec)); // default, sequential
sort(std::execution::par, begin(vec), end(vec)); // parallel
```
26. Multiple `shared_ptr` instances of an object "share" the ownership. The object will be destroyed when "the last shared pointer" is destroyed (Ch.13 p.165). Unique pointers are generally more efficient because it does not need to track the "use count" that shared pointers do.
27. STL containers' "moved-from state" is "empty" (Ch.13 p.168).
28. There is no time/space overhead to use `std::array` versus the built-in C-style array (Ch.13 p.171).
29. The second template argument (size) of `std::array` must be a constant expression (Ch.13 p.171):
    ```cpp
    int n = 3;
    std::array<char, n> arr{'a', 'b', 'c'}; // error: size not a constant expression
    ```

    - We need to specify n as a "const" or a "constexpr" here, like so:

        ```cpp
        const int n = 3;
        std::array<char, n> arr{'a', 'b', 'c'}; // good

        constexpr int n = 3;
        std::array<char, n> arr{'a', 'b', 'c'}; // good
        ```

    - Note that we cannot use a function parameter as the size of the array, because function parameters are NEVER constant expressions:

        ```cpp
        void f(const int n) {
          std::array<char, n> arr{'a', 'b', 'c'}; // error!
        }

        void f(constexpr int n) { // invalid constexpr!
          std::array<char, n> arr{'a', 'b', 'c'};
        }
        ```

    - To solve this, we must make a template like so:

        ```cpp
        template <int n>
        auto func2() -> std::array<int, n> {
          std::array<int, n> a{1, 2, 3};
          return a;
        }

        int main() {
          auto a = func2<3>();
          // ...
        }
        ```


30. `std::array` can be passed to C-style function that expects a pointer, using `.data()`, or with the explicit address of its first element (Ch.13 p.171):

    ```cpp
    void func1(int *p, int sz);
    
    void func2() {
      const int n = 3;
      std::array<int, n> a{1, 2, 3};
      func1(a, n);         // ERROR!
      func1(&a[0], n);     // good
      func1(a.data(), n);  // good
    }
    ```
31. Advantages of `std::array` over built-in C-style array include (Ch.13 p.172):
   - Ease of use with STL algorithms.
   - Can be copied using `=`.
   - No surprising conversions to pointers.
32. "Template argument type deduction from constructor arguments" is added in C++17 (Ch.13 p.174):

    ```cpp
    tuple<string, int> tup1{"Ian Pan", 123}; // pre C++17
    tuple              tup2{"Ian Pan", 123}; // post C++17
    // Alternatively...
    auto tup3 = make_tuple("Ian Pan", 123);
    ```
    - Getting an element from a tuple can be done as follows:
      ```cpp
      auto s1 = get<0>(tup1);
      auto s2 = get<string>(tup1);
      ```
    - We also use `get<>()` to WRITE to the tuple:
      ```cpp
      get<0>(tup1) = "Columbia";
      get<string>(tup1) = "University";
      ```

33. An `optional<T>` can be seen as a special kind of variant, like a `variant<T, nothing>` (Ch.13 p.176).
34. `std::enable_if` is widely used for conditionally introducing definitions (e.g. for defining an operator member function) (Ch.13 p.184).
35. (Ch.14 p.192) To generate random numbers, one can specify the "engine", the "distribution" (e.g. uniform, Gaussian, exponential, etc.) and create a "generator" out of it:

    ```cpp
    using my_engine = default_random_engine;
    using my_distribution = uniform_int_distribution<>;
    my_engine eng{};
    my_distribution dice{1, 6};
    auto rollDice = [&]() { return dice(eng); };
    
    auto num = rollDice();
    ```

36. `std::valarray` is a vector-like container that supports element-wise mathematical operations, as well as slicing and shifting (rolling). (Basically, the superpowers we thought was unique to Python) (Ch.14 p.192)
37. Word of advice: "Consider `accumulate()`, `inner_product()`, `partial_sum()`, and `adjacent_difference()` before you write a loop to compute a value from a sequence" (Ch.14 p.193).
