---
title: "Cpp Type Inference(auto and decltype)"
date: 2022-10-08 +0800
categories: [C++]
tags: [c++11]
---

Type inference is to automatic deduction of the data type of an expression. Since I already use `auto` a lot, so this post will mainly focus the explaination (not the usage) of `decltype`, and also mention other two related words `typeid` (an operator) and `typeof`(not in C++, only supported in compiler GCC).

## Keyword auto

**auto** , use it to deduce of the (static) type of an object from its initializer.
It bring much convenice to avoid writing long and complex class type name, but overuse of it also cause the code not easy to read, so there's rule from C++ Core Guildelines

> ES.11: Use auto to avoid redundant repetition of type names
{: .prompt-tip }

```cpp
auto n = 1; // OK n is an int
auto x = make_unique < Gadget >( arg ); // OK : x is a std :: unique_ptr < Gadget >
auto y = flopscomps (x ,3); // Bad : what might flopscomps () return ?
```

> The variable declared with auto keyword should be initialized at the time of its declaration only or else there will be a compile-time error.
{: .prompt-info}

```cpp
auto a ; // error, 'a' is not initialized at declaration time
auto b=4; // ok, b is an int
```

> `auto` won't keep the reference semantics, even if one variable is assigned with an reference, to keep the reference, use `auto&` , or decltype.
{: .prompt-warning}

```cpp
#include <iostream>
#include <string>
using namespace std;

int& foo(int& x)
{
    return x;
}

int main()
{
    auto a = 1;
    auto& b =a;
    auto c = b; // c is int or int& ?
    auto d = foo(a);
    auto& e =foo(a);

   // cout << typeof(a)<<'\n';
    cout << "type id of b :"<<typeid(b).name()<<'\n';
    cout << "type id of c :"<<typeid(c).name()<<'\n';
    cout << "type id of d :"<<typeid(d).name()<<'\n';
    cout << "type id of e :"<<typeid(e).name()<<'\n';
    return 0;
} 
```
Output (tested on https://godbolt.org/z/bhT83creq , should also try it later on ubuntu , gcc9)
```
type id of b :i
type id of c :i
type id of d :i
type id of e :i
```
## Keyword decltype
**decltype** is short for *declared type*, it's a replacement if `typeof`. It's introduced to keep the reference semantics, as `auto` cannot. 'auto' lets you declare a variable with a particular type, whereas `decltype` lets you extract the type from the variable (or an expression). So `decltype` is more like an operator.

```cpp
// here only provide simple syntx of decltype, as it's mainly use in template metaprogramming

// C++ program to demonstrate use of decltype
#include <bits/stdc++.h>
using namespace std;
 
int fun1() { return 10; }
char fun2() { return 'g'; }
 
int main()
{
    // Data type of x is same as return type of fun1()
    // and type of y is same as return type of fun2()
    decltype(fun1()) x;
    decltype(fun2()) y;
 
    cout << typeid(x).name() << endl;  //output :i
    cout << typeid(y).name() << endl;  //output :c
 
    return 0;
}
```

> The primary purpose/ usage of `decltype` is to to enable sophisticated metaprogramming in foundation libraries.
{: prompt-info}


A program that demonstrates use of both auto and decltype 
Below is a C++ template function min_type() that returns the minimum of two numbers. The two numbers can be of any integral type. The return type is determined using the type of minimum of two.

```cpp
// C++ program to demonstrate use of decltype in functions
#include <bits/stdc++.h>
using namespace std;
 
// A generic function which finds minimum of two values
// return type is type of variable which is minimum
template <class A, class B>
auto findMin(A a, B b) -> decltype(a < b ? a : b)
{
    return (a < b) ? a : b;
}
 
// driver function to test various inference
int main()
{
    // This call returns 3.44 of double type
    auto min_d = findMin(4,3.44);
    cout <<typeid(decltype(min_d)).name() <<":"<< min_d << endl;
 
    // This call returns 3 of double type (not understand why the return type is d?)
    auto min_i = findMin(5.4,3);
    cout << typeid(min_i).name() << ":" << min_i << endl;
 
    return 0;
}
```

Output: 
```
d:3.44
d:3
```
## Operator typeid

Used where the dynamic type of a polymorphic object must be known and for static type identification.
To use `typeid`, must `#include <typeinfo>`

## typeof
`typeof` is neither a keyword nor an operator in C++ lanuage, it's macros and compiler extensions in GCC. In c++ 11, `decltype` is the replacement of `typeof`.

The primary difference between the two is the following
- `typeof` is a compile time construct and returns the type as defined at compile time
- `typeid` is a runtime construct and hence gives information about the runtime type of the value.

## References
- https://www.geeksforgeeks.org/type-inference-in-c-auto-and-decltype/
- https://www.stroustrup.com/hopl20main-p5-p-bfc9cd4--final.pdf
- https://en.cppreference.com/w/cpp/language/typeid
- https://stackoverflow.com/questions/1986418/typeid-versus-typeof-in-c