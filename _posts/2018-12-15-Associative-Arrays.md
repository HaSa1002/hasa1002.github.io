---
title: "[C++] #1 New series and associative arrays"
date: 2018-12-15 22:00:00
tags: C++ tricks
layout: default
comments: true
---

# #1 New series and associative arrays #
Welcome back. The last article was published almost six months ago. During this long period I created a few things and spent most of my time into my different hobbies. Sadly this blog got a shortage since I could not get me up to write a few new articles.
In this series I want to give insights in my thoughts and design and programming patterns to help you to use my code. I am also planning to spend some time writing about different topics in the C++ universe. If you have wishes feel free to ask in the comments below.
## Associative Arrays ##
In C++ we normaly do not have assosiative Arrays. In most cases we just do not need them and hence we do not care. But as always there are a few cases when I think using them is benifical, especially regarding code readablility:

1. You have a set of variables which belong together
2. You do something like that:
```cpp
    std::array<bool, 5> arr;
    arr[0] = true; //Test
    arr[1] = false; //sts
    //..

```

As I stated above I usally use them to improve code readability. However there are different methods to achieve this. I will show how to use them and when to decide for what method. I will only show methods with the normal std library. For the sake of demonstation, all value types are `bool`.

### The easy way ###
The easiest way is probably this one. We use a a `std::unordered_map<std::string, bool>` defined in the `<unordered_map>` header. This will end in a syntax like this:
```cpp
    #include <unordered_map>;

    std::unordered_map<std::string, bool> arr {
     {"test", true},
     {"sts", false},
     //...   
    };

    // We just simply access the array:
    arr["test"];
    arr["sts"] = true;
    arr["new"] = true;
    //...

```

However there is a downside. As we use a [unordered map](https://en.cppreference.com/w/cpp/container/unordered_map) the hash table has to be rehashed, because of the internal implementation. The other effect is, since we use strings, that the map consumes a lot of space. You need to decide if the better readability is worth the performance cost. (As I wrote this post, I hadn't sufficient data to prove the performance side. It's highly speculative, but would meet my understanding of this container. Please prove me wrong, if I am. I will add some prove or kill this paragraph once I finished all the tests.).

### The hard way ###
The hard way is to create a `enum` and use it in a normal `std::array` however this is not as free as the example above.
```cpp
#include <array>

enum items {
    first,
    second,
    test,
    //...
    
    SUM
};
std::array<bool, SUM> arr;
arr[first] = true;
arr[second] = true;

```

Keep in mind to NOT use a `enum class` since it does not express an integer value, which is required in order to use an array.

### Which one do I use? ###
It really depends. If I need a flexible version or the name of the entry is of importance, I use the first approach. For example I use it in my [xml-filestreams libraray](https://github.com/HaSa1002/xml-filestreams), as I read the attributes of a tag and since I do not know at compile-time, if what attribute names are set I have to use the first one. For the example given at the top on the other hand I would use the second, because I know at compile time how many entries to expect.
Remember:
If you know at compile-time how many items to expect go for the faster `std::array`-approach. However if you do not know use the first. If you do not need the the entries to be written anywhere, you could instead use `std::unordered_map<size_t, bool> arr` and hash the strings with `std::hash<std::string> hs` this way:

```cpp
arr[hs("one")] = true;
arr[hs("two")] = false;
arr[hs("new")] = true;
//...
```
