---
layout: page
title: F#
permalink: /fsharp/
---
# Functional programming in F#

## Week 1

### Recursive functions
**Material:** HR 1, HR 2.1-2.6, HR 4.1-4.3 <br>
**Exersices:** http://www2.compute.dtu.dk/courses/02157/ExercisesWeek01.pdf

```fsharp
(1) int * int -> int

let rec bin(n,k) =
    match n,k with
    | (_,0) -> 1
    |  _,_ when n = k -> 1
    | (n,k) -> bin(n-1, k-1) + bin(n-1, k)
```
(1) The function takes two integer parameters and returns an integer. 

The input-matching syntax allows us to match input with certain criteria. If, for instance, `n` is **any** number and `k = 0`, the input is matched with `1`. If `(n,k)` amounts to any number when `n = k`, the input is matched with 1. For any other values `(n,k)`, the function with match them with `bin(n-1, k-1) + bin(n-1, k)` which is a recursive callback based on the mathematical criteria for binomial coefficients in Pascal's triangle. As such, this function returns exactly that.

```fsharp
(1)'a *' a list -> int

let rec multiplicity(x, ys) =
    match ys with
    | [] -> 0
    | ys::tail when ys = x -> 1 + multiplicity(x,tail)
    | ys::tail -> multiplicity(x,tail)
```

(1) The function accepts a type `a`, a list of type `a` and returns an integer.

It computes the multiplicity of a value in an array or list of values of the same type. This means that the function is able to find the multiplicity of anything from type `bool` to type `int`.
The `tail` is the remainder of the list, which is an immutable type. The head always points at the tail, and the head will always be the current value. As the tail is passed in the recursive callback `multiplicity(x,tail)`, the next iteration will then treat the first element as the head and the remainder as its tail. This keeps going until the last element of the list, which will pass an empty tail and match the criteria `[] -> 0`. Whenever the head matches the value `x`, the multiplicity is increased by 1.

___
## Week 2

### Functions and tuples
**Material:** HR 2, HR 3.1-3.3, HR 4.1-4.4 <br>
**Exersices:** Paper and pensil exercises HR 4.16, 4.17

The wildcard pattern can be used in tuple patterns. Every value matches this pattern, but the matching provides no bindings. For example:

```fsharp
let ((_,x),_,z) = ((1, true), (1, 2, 3), false);;
val z : bool = false
val x : bool = true
```

A **record** is a generalized tuple where each component is identified by a **label** instead of a position in the tuple. Note that the attributes of `john : person` are not set in the order they were first declared.

```fsharp
type person = {name : string; age : int; bd : int*int}
let john = {bd = (2,3); age = 20; name = "John"}
```

Note that we can generate bindings of the identifiers by matching the person record as such: 

```fsharp
let {name = x; age = y; bd = (d,m)} = john;;
val y : int = 20
val x : string = "John"
val m : int = 3
val d : int = 2
```

___

## Week 3

___



