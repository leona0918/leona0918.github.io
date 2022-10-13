---
title: "Enum and std::bitset Usage in C++"
date: 2022-10-13 +0800
categories: [C++]
tags: [c++11]
---

There're two common practices to define enumeration type, explicitly give enum type an integer, or not assign integer, the underlying value of enum will increase one by default. Both enumerations can be used with bitset type, which could enumeration operation more easier.

## Define Enum Not Asign Integer

The value of first element is 0, then next element's value auto increase by 1. Use the last element as the number of all elements. The total valide labels count is 5.
```cpp
enum Labels { tall, sshort, small, large, medium, numLabels };
```

## Define Enum with Unsigned Integer
This way of define enum, could use binay operation on this type.

``` cpp
enum Flags : std::uint16_t {
  kNoneFunction = 0,   // 0000 0000
  kHeat = 1 << 0,      // 0000 0001
  kMetal = 1 << 1,     // 0000 0010
  kSmallSize = 1 << 2, // 0000 0100
  kHeavy = 1 << 3,     // 0000 1000
  kBrand = 1 << 4      // 0001 0000

};
```

## What is bitset

It's  a fixed-size sequence of N bits, the size should be decide on compile time


## Reference
 - http://www.java2s.com/Tutorial/Cpp/0360__bitset/Usebitsetwithenumtogether.htm
 - https://en.cppreference.com/w/cpp/utility/bitset

## Sample Code

``` cpp

/*
This example is to show the usage of enum and bitset
Can be comiled with command `g++ -o enum.exe filename.cpp`
Run it with `enum.exe`
*/
#include <bitset>
#include <iostream>

enum Labels { tall, sshort, small, large, medium, numLabels };

void labels_test() {
  std::bitset<Labels::numLabels> labels;
  labels.set(Labels::tall);
  // labels.set(Labels::small);

  std::cout << labels << std::endl;
}

enum Flags : std::uint16_t {
  kNoneFunction = 0,   // 0000 0000
  kHeat = 1 << 0,      // 0000 0001
  kMetal = 1 << 1,     // 0000 0010
  kSmallSize = 1 << 2, // 0000 0100
  kHeavy = 1 << 3,     // 0000 1000
  kBrand = 1 << 4      // 0001 0000

};

void flags_test() {
  std::bitset<sizeof(std::uint16_t) * 8U> flags;
  flags = (Flags::kBrand | Flags::kSmallSize);

  //flags.set(Flags::kBrand); // error usage, which would cause run time out of range error

  printf("the number of valid flags is %ld\n", flags.count());
  std::cout << flags << std::endl;
}

int main() {

  labels_test();
  flags_test();
  printf("hello world\n");
  return 0;
}
```