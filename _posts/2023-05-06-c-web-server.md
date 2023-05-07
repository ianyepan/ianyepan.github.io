---
title: Building a Simple Web Server in C - an open source project
author: Ian Y.E. Pan
date: 2023-05-06 21:30:00 -0400
categories: [Programming]
tags: [programming]
---

This [Github
repo](https://github.com/bloominstituteoftechnology/C-Web-Server) is a
well-designed small project to learn about web servers in C. A lot of
the heavy lifting data structures and basic interactions using the
socket API have been given to us. The focus of this project is to
build an HTTP request parser, an HTTP response builder, and an LRU
cache consisting of a doubly linked list and a hash
table. Understanding the given code is also pretty much a
prerequisite, otherwise it'd be hard to understand the code
abstractions and the instructions on what to implement.

The given skeleton code is well-documented, and the README even
includes some background reading for networking and LRU caches.

All in all, it's a nice little project that takes no longer than one
or two evenings to finish (provided that your know C well).

In this blog post, I want to talk about some of the challenges to look
out for, funny bugs I made, and how I solved them.

## Caveats when sending header and body of a HTTP response

One of the first tasks in this project is to complete the function
`send_response` with the following prototype:

```c
int send_response(int fd, char *header, char *content_type, void *body, int content_length);
```

where `fd` is the listening socket returned by the `socket()` function
from `sys/socket.h`.

We want to create a response and send it using the `send()` function
(again, from `sys/socket.h`). The response has the following format:


```log
HTTP/1.1 200 OK
Date: Sat May 6 21:05:11 PST 2023
Connection: close
Content-Length: 31740
Content-Type: text/html

<!DOCTYPE html><html><head><title>Example page ...
```

I immediately thought of constructing the response string using
`snprintf()`, which is the closest thing we get in C that's roughly on
par with C++'s stringstream, or Python's f-string.

I coded the following solution, but the response would only be
correctly received by localhost if the response body is plain text (or
html). If it were a PNG image, the page would crash.

```c
...
response_length = snprintf(response,
                           max_len,
                           "%s\n"
                           "Date: %s"
                           "Connection: close\n"
                           "Content-Length: %d\n"
                           "Content-Type: %s\n"
                           "\n%s\n",
                           header, date_string, content_length, content_type, (char *)body);
                           
rv = send(fd, response, reponse_length, 0);

if (rv < 0) {
    ...
}
...
```

Could you see the problem? I was treating `void *body` as a string and
thought I could simply format it as part of my response, which itself
is a `char *`.

However, If we try to copy binary data (such as a PNG file) directly
into a string using `snprintf()`, it will stop at the first occurrence
of the null character, truncating the data and potentially corrupting
it. One way to solve it is to send the header and body separately,
like so (I'm sort of abusing the term "header" here because the
`header` variable technically only contains the first line of the
header, which is `HTTP/1.1 200 OK` or `HTTP/1.1 404 NOT FOUND`. While
`header_length` is the size of the full header, including content
type, content length, and date, etc.):


```c
header_length = snprintf(response, max_response_size,
                         "%s\n"
                         "Date: %s\n"
                         "Connection: close\n"
                         "Content-Length: %d\n"
                         "Content-Type: %s\n"
                         "\n",
                         header, date_string, content_length, content_type);

rv = send(fd, response, header_length, 0);

if (rv < 0) {
    ...
}

rv = send(fd, body, content_length, 0);

if (rv < 0) {
    ...
}
...
```


Alternatively, we could use `memcpy()` to "force" append the full binary data
as part of the response, without having it stop at the first
occurrence of a null character. In other words, `memcpy()` is a
function that copies a specified number of bytes from one memory
location to another, regardless of their contents. It does not care if
the data being copied contains null characters or any other byte
value.

The `memcpy()` method would look as such:

```c
header_length = snprintf(response, max_response_size,
                         "%s\n"
                         "Date: %s\n"
                         "Connection: close\n"
                         "Content-Length: %d\n"
                         "Content-Type: %s\n"
                         "\n",
                         header, date_string, content_length, content_type);

memcpy(response + header_length, body, content_length);

rv = send(fd, response, header_length + content_length, 0);

if (rc < 0) {
    ...
}
```

Note that the destination of `memcpy()` (the first argument) is the
end of `response`, effectly appending the full body right after
header. Also note that an extra new line character is needed between
the header and the body (two `'\n'` in a row).

## Caution: Always initialize your struct members!

I ran into a seemingly weird bug when running several unit tests in a
row. Each unit test is supposed to free the "cache" when it's done
checking its own set of assertions, and the next unit test would
instantiate a new cache for further checks.

My unit tests keep failing because later unit tests would "remember"
the size (which is a member of `struct cache`) of the last freed cache
in the previous unit test. How could that be? We are freeing the cache
every time a unit test finishes! I checked my `free_cache()`
implementation and it looked fine. After an hour of debugging, I
finally realized that I forgot to initialize the size of the cache
when I created the cache.

What happened was that the cache in the previous unit test was
correctly freed, and the C compiler was smart enough to allocate the
new cache in the next unit test at the exact same memory address as
before. But since I forgot to initialize the `size` of `struct cache`
during instantiation, the `size` member carried over the value of the
previous `size`. Had I, for whatever reason, reset the `size` to zero
before I freed the cache, then I might never have found the bug during
unit testing (which would definitely backfire some other time). A
blessing in disguise, I suppose.

## Free your malloc'd variables, and nothing else

Our cache structure looks as follows:

```c
struct cache_entry {
    char *path;
    char *content_type;
    int content_length;
    void *content;

    struct cache_entry *prev, *next;  // Doubly-linked list
};
```

In this project, cache entries are dynamically allocated and
initialized like so:

```c
struct cache_entry *alloc_entry(char *path, char *content_type, void *content, int content_length)
{
    struct cache_entry *new_entry = malloc(sizeof(*new_entry));

    if (!new_entry) {
        ...
    }

    new_entry->path = path;
    new_entry->content_type = content_type;
    new_entry->content = content;
    new_entry->content_length = content_length;
    new_entry->prev = NULL;
    new_entry->next = NULL;

    return new_entry;
}
```

Notice that we are not dynamically allocating the string members of
`struct cache_entry`, but rather just the struct itself. This means
that when we want to free the cache entry, we just had to call
`free(cache_entry)` and do not need to first free its members. If
instead we explicitly tried to free the members, the compiler will
throw an error saying that we attempted to free an invalid pointer.

However, if the members were indeed malloc'd, then it's vital that we
free the struct members first before we free the struct. Among the
struct members, it generally does not matter which order we free
them. However, the best practice would be that we free the members in
the reversed order of how they were allocated.

## A few words before you start...

If you plan to do this project (which I highly recommend -- won't take
longer than a couple of hours), I'd like to give you a head start on
the given skeleton code.

In general, there's only one cache structure when the program is
running, and the cache has one hash table, and one doubly linked
list. The struct is defined as follows:

```c
struct cache {
    struct hashtable *index;          // Hash table
    struct cache_entry *head, *tail;  // Doubly-linked list
    int max_size;                     // Maxiumum number of entries
    int cur_size;                     // Current number of entries
};
```

The hash table is inconveniently named `index`, which might confuse
some people as we normally thought of an index as a number. Here, the
index rather has the meaning of "record". The cache, with its hash
table and doubly linked list, stores multiple cache "entries". The
doubly-linked list is useful to quickly remove the tail, move an entry
to the head, or insert a new head, all frequent operations we would do
with an LRU cache. The hash table is helpful to swiftly look up the
entry pointer given a request file path (serving as the key to the
hash table).

- When constructing the response, `snprintf()` might come in handy.

- When parsing the request, `sscanf()` might come in handy.

- To determine the content type (MIME type), the skeleton code has a
  handy function: `mime_type_get()` which takes a file path, extracts
  and maps its extension to the correct MIME type.
  
- Here's a helper function I wrote to format the date of the response header:
```c
  // #include <time.h>

  static inline void populate_date_string(char *buf, size_t max_len)
  {
      time_t rawtime = time(NULL);
      struct tm *timeinfo = localtime(&rawtime);
  
      strftime(buf, max_len, "%a, %d %b %Y %H:%M:%S %Z", timeinfo);
  }
```
To use it, you should specify a char array with maximum date length
and pass it into this void function.

## Conclusion

C-Web-Server is a fun little project to practice C programming while
reviewing concepts such as LRU cache, memory management, and the HTTP
protocol. I hope you enjoyed reading this post, and are maybe thinking
about finishing this project yourself!
