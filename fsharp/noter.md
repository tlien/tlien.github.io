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
The `tail` is the remainder of the list, which is an immutable type. The head always points at the tail, and the head will always be the current value. As the tail is passed in the recursive callback `multiplicity(x,tail)`, the next iteration will then treat the first element as the head and the remainder as its tail. This keeps going until the last element of the list, which will pass an empty tail and match the criteria `[] -> 0`. Whenever the head matches the value `x`, the multiplicity is increased by 1. More on lists below.

___ 
<br>

## Week 2

### Functions, tuples and lists
**Material:** HR 2, HR 3.1-3.3, HR 4.1-4.4 <br>
**Exersices:** Paper and pensil exercises HR 4.16, 4.17

A **list** is comprised of its head and tail, marked x_0::[x_1; x_2 ... x_n]. The infix operator `::` (called cons) builds a list from its head and its tail, for instance 

```fsharp
let x::xs = [1; 2; 3; 4; 5; 6];;
val xs : int list = [2; 3; 4; 5; 6]
val x : int = 1
```

It is also possible to compose a list as such:

```fsharp
let x = 1::2::3::[4;5;6;7];;
val x : int list = [1; 2; 3; 4; 5; 6; 7]
```
While the below pattern binds `x` to 1, `y` to 2, `z` to 3 and `xs` to [4;5;6]. Hence the whole list is bound to `x::y::z::xs` 

```fsharp
let x::y::z::xs = [1; 2; 3; 4; 5; 6];;
val z : int = 3
val y : int = 2
val xs : int list = [4; 5; 6]
val x : int = 1

let list = x::y::z::xs;;
val list : int list = [1; 2; 3; 4; 5; 6]
```
Note the different roles of the operator symbol `::` in above expressions. It denotes decomposing a list into smaller parts when used in a pattern like `x0::x1::xs`, and it denotes building a list from smaller parts in an expression like `0::[1;2]`

Lists can only hold the same type. They may also hold pairs of any type, for instance ints:

```fsharp
let (x,y)::z::xs = [(1,2);(4,5);(7,8);(10,11)];;
val z : int * int = (4, 5)
val y : int = 2
val xs : (int * int) list = [(7, 8); (10, 11)]
val x : int = 1
```
or strings:

```fsharp
let (x,y)::z = [("Hi", "there");("Hi", "there");("Hello", "world")];
val z : (string * string) list = [("Hi", "there"); ("Hello", "world")]
val y : string = "there"
val x : string = "Hi"
```
and the same goes for any other type, even records:

```fsharp
let fam = [{name = "Mom"; age= 55}; {name = "Dad"; age = 56}];;
val fam : person list = [{name = "Mom";
                          age = 55;}; {name = "Dad";
                                       age = 56;}]
```

#### **List range expresssions**

The range expression is a special construct that allows us to construct lists from patterns like `[b..e]` and `[b..s..e]` where `b, s, e` are number types. `[b..e]` is called the _range_ expression, creating a list ranging from `b` to `e`. The numbers may be positive or negative.

```fsharp
let l = [1..5];;
val l : int list = [1; 2; 3; 4; 5]
```

the `s` in the range expression is called the _step_. The step is either ascending or descending, depending on the sign of `s`, however `s` may never be 0.
For instance, the ascending list of integers in steps of 5 from 1-16 is

```fsharp
let l = [1..5..16];;
val l : int list = [1; 6; 11; 16]
```

#### **Pattern matching on result of recursive call**

We can split the result of a recursive call into components using pattern matching. The function _sumProd_ computes the pair consiting of the sum and the product of the elements in a list of integers.

```fsharp
let rec sumProd = function
    | []    -> (0,1)
    | x::rest ->
           let (rSum, rProd) = sumProd rest
           (x+rSum, x*rProd);;

sumProd [2; 5; 7];;
val it : int * int = (14, 70)

```
We can declare a function _mix_ that mixes the elements of two lists of the same length and type.

```fsharp
let rec mix = function
    | (x::xs, y::ys) -> x::y::(mix (xs, ys))
    | ([], []) -> []
    | _         -> failwith "mix: parameter error";;

mix ([1;2;3], [3;2;1]);;
val it : int list = [1; 3; 2; 2; 3; 1]
```

### **Polymorphism**


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
<br>

## Week 3

___



