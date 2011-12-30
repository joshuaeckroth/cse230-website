---
title: Homework 3 Alternative - Stack calculator
layout: default
---

Your task is to program a calculator that uses a stack. These
calculators use "postfix" notation or "Reverse Polish notation (RPN)"
for their mathematical expressions.

For example, `5 - 2` is instead written `5 2 -` and `5 - (2 + 1)`
written `5 2 1 + -`

The way this works is as follows:

  * If a number is typed, push it on the stack.

  * If an operation is typed, depending on the operation, pop off zero
    or more numbers from the stack, perform the operation, and
    possibly push the result(s) back on the stack.

