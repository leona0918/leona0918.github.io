---
title: "Static Assert vs Assert"
date: 2022-10-10 +0800
categories: [C++]
tags: [c++11]
---

## static_assert (compile time assertion)
`static_assert` is good for testing logic in your code at compilation time.
## assert
`assert` is good for checking a case during run-time that you expect should always have one result, but perhaps could somehow produce an unexpected result under unanticipated circumstances.  For example, you should only use assert for determining if a pointer passed into a method is null when it seems like that should never occur. `static_assert` would not catch that.

## Assert compare to `if`
`assert` can be used to break program execution, so you could use an if, an appropriate error message, and then halt program execution to get a similar effect, but `assert` is a bit simpler for that case. `static_assert` is of course only valid for compilation problem detection while an if must be programmatically valid and can't evaluate the same expectations at compile-time. (An if can be used to spit out an error message at run-time, however.)

## Reference 
- https://en.cppreference.com/w/cpp/language/static_assert
- https://stackoverflow.com/questions/18210270/what-is-the-difference-between-assert-and-static-assert