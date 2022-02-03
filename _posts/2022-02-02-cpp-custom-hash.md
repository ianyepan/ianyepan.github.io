---
title: Custom Hash Functions for C++ Unordered Containers
author: Ian Y.E. Pan
date: 2022-02-02 13:44:00 -0500
categories: [Programming]
tags: [programming, tips]
---

> For simplicity, I'll be "using namespace std" throughout this post. In production code, one should refrain from such pollution of namespace, though.

## Motivation and Background

C++ unordered containers (e.g. `unordered_map`, `unordered_set`, etc.) uses "hashing" to store objects. The STL provides hash functions for commonly used types, like `string` and numeric values such as `int`, meaning that we won't have to provide any hash functions explicitly when creating an unordered container instance:

```cpp
unordered_set<string> names{"Ian", "Y.E.", "Pan"}; // good
```

However, when we want to hold a more complex type, or even a user-defined class, C++ unordered containers would fail to find a hash function for itself to use. This is when custom hash functions come in. We will have to explicitly define a hash function for the type (e.g. `pair<string, int>`) that we want to use as a template argument for some unordered container. 

## Implementation Idea: Exclusive-Or

According to the book "A Tour of C++" by Bjarne Stroustrup, "A hash function is often provided as a function object". And creating a new hash function by "combining existing functions using exclusive-or (`^`) is simple and often very effective".

Intuitively, this means that if we want to hash a pair of string and integer, we could use the following "new" hash function:

```cpp
// p is a pair<string, int>
hash<string>{}(p.first) ^ hash<int>{}(p.second); // simple and effective
```

The next thing we need to do is to pass that as a template argument when creating our unordered container. For an `unordered_set`, that will be the second argument. For an `unordered_map`, that'll be the third argument after the key type and the value type. Note that we only need the hashing function for a map's "key", not the value.

## Method 1: Function Object with Struct

Perhaps the most commonly used method (and also my preferred method) is to create a function object (functor) using a struct, and implement its `operator()` function. This function is to be qualified as `const`, and takes a const reference to the custom type and returns a `size_t`. Defining a function object outside as a standalone struct also gives it the flexibility of "generic programming" with templates. Meaning that it can work with pairs of different template arguments, not just strings and ints. 

Putting this all together, we have:

```cpp
struct PairHash {
  template <typename T1, typename T2>
  auto operator()(const pair<T1, T2> &p) const -> size_t {
    return hash<T1>{}(p.first) ^ hash<T2>{}(p.second);
  }
};

class Example {
 public:
  void func() {
    unordered_set<pair<string, int>, PairHash> uset;
    unordered_map<pair<double, string>, int , PairHash> umap;
  }
};
```

## Method 2: Custom Lambda Hash Function

Alternatively, we can also implement our custom hash function as a lambda. Some may find this inline implementation more elegant. A drawback is that C++11 lambdas do not support templates. Fortunately, we have "generic lambdas" since C++14 (as seen below). 

Another drawback is that we need to use `decltype` of the lambda as the extra custom hash function argument for our unordered containers, and also provide constructor arguments in parentheses after the declared variable name. Both `unordered_set` and `unordered_map` have a constructor that takes an initial number of buckets and a hashing object as inputs. Here, I just arbitrarily let the initial buckets be 10.

```cpp
class Example {
 public:
  void func() {
    auto pairHash = []<typename T1, typename T2>(const pair<T1, T2> &p) {
      return hash<T1>{}(p.first) ^ hash<T2>{}(p.second);
    };
    unordered_set<pair<int, int>, decltype(pairHash)> seen(10, pairHash);
    unordered_map<pair<double, string>, int , decltype(pairHash)> umap(10, pairHash);
  }
};
```

## Method 3: Defining a Specialization of std::hash()

In this method, we add a specialization of standard-library's hash function to the namespace `std`. This is quite similar to the first method, except now we must use `hash` as the name of our function object. We also have to specify the template class for which we are defining the specialization. The immediate advantage is that we do not need to pass in any extra template arguments to our unordered container when declaring an instance.

Here is an example of this method, suppose we want to be able to hash a user-defined class `XYZ`, which has a member called `value`:

```cpp
namespace std {
template <>
struct hash<XYZ> {
  auto operator()(const XYZ &xyz) const -> size_t {
    return hash<XYZ>{}(xyz.value);
  }
};
}  // namespace std

unordered_set<XYZ> xset;
```

## Overloading operator==()

Note that the unordered containers also need the ability to compare two different keys using `==`. Throughout this post, I have been using `std::pair`, and the STL already defines `operator==()` for pairs, so I didn't have to define my own equality logic. However, if we are using our own user-defined class, then we need to define the equality operator. For example, here's how we can define `operator==()` for our custom class `Record`:

```cpp
class Record {
 public:
  // ...
  bool operator==(const Record &other) const {
    return id == other.id; // no problem accessing other.id
  }
  int getId() const {
    return id;
  }

 private:
  int id;
  string name;
};
```

Note that `operator==` can access private members "id" of "another" Record object, because it is a "member function". Member functions can access private members of its class, not only for `this`, but also for any instance of the same class. To read more on this topic, consider [this StackOverflow thread](https://stackoverflow.com/questions/12479875/why-overloading-can-access-private-members-of-argument).

With `Record`'s equality operator defined, we can proceed to define our custom hash function for `Record`. Let's use the third method for this example.

```cpp
namespace std {
template <>
struct hash<Record> {
  auto operator()(const Record &record) const -> size_t {
    return hash<int>()(record.getId());
  }
};
}  // namespace std
```

Now, we can finally use `Record` as the key to our unordered container, simply as follows:

```cpp
unordered_set<Record> recordsSet;
unordered_map<Record, int> recordsMap;
```

## Conclusions and Further Readings

In this short blog post, I introduced various ways to create custom functions for unordered containers that hold relatively complex STL types or user-defined class objects. If you'd like to read more on this topic, I recommend [this blog post](https://marknelson.us/posts/2011/09/03/hash-functions-for-c-unordered-containers.html) by Mark Nelson and also [this page](https://en.cppreference.com/w/cpp/utility/hash) at Cppreference.com.