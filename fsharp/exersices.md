---
layout: page
title: F# Exersices
permalink: /fsharp/exersices
---

## Week 1

```fsharp
// 1.
let rec f n = if n > 0 then n + f(n-1) else 0


// 2
let rec sum(m,n) = 
    match m,n with 
    | _,0 -> m
    | 0,_ -> 0
    | m,n -> m + n + sum(m, n-1)

// 3
let rec bin(n,k) =
    match n,k with
    | (_,0) -> 1
    |  _,_ when n = k -> 1
    | (n,k) -> bin(n-1, k-1) + bin(n-1, k)

// 4
let rec multiplicity(x, ys) =
    match ys with
    | [] -> 0
    | ys::tail when ys = x -> 1 + multiplicity(x,tail)
    | ys::tail -> multiplicity(x,tail)

// 5
let rec mulC(x, ys) =
    match ys with
    | [] -> []
    | y::tail -> (y*x)::mulC(x, tail)

// 6
let rec addE(xs, ys) =
    match xs, ys with
    | _, [] -> xs
    | [], _ -> ys
    | x::xtail, y::ytail -> (x + y)::(addE(xtail, ytail))

// 7
// A
let rec mulX(ys) = 
    match ys with
    | ys -> 0::ys

// B
let rec mul(xs, ys) = 
    match xs, ys with
    | [],_ -> []    
    | x::xtail, ys -> addE(mulC(x,ys), mul(xtail, mulX(ys)))
```
<br>

___

<br>

## Week 2

```fsharp
// 2.1
let div = function
    | x when x % 5 = 0 -> false
    | x when x % 2 = 0 -> true
    | x when x % 3 = 0 -> true
    | _ -> false

// 2.2
let rec stringCon = function
    | (_,n) when n <= 0 -> ""
    | (s,n) -> s + stringCon(s,n-1)

// 4.3
// All four solutions yield the same result
let evenEZ n = [2..2..n*2]

let evenN = function
    | n when n >= 1 -> [2..2..n*2]
    | _ -> []

let evenN2 n = 
    [for x in 2..2..n*2 do yield x]

let evenN3 n = List.filter (fun y -> y%2=0) [1..n*2]

// 4.8
let rec split = function
    | x1::x2::rest -> 
            let (a,b) = split rest 
            (x1::a, x2::b)
    | _ -> ([],[])

// 4.9
let rec zip = function
    | x1::xrest, y1::yrest ->
            let a = zip (xrest, yrest)
            (x1,y1)::a
    | _ -> []

// 4.12
// NOT YET SOLVED
let p = fun x -> x > 2;

let rec sum = function
    | (p,x::rest) when p(x) -> 1 + sum(p,rest);
    | _ -> 0


// 4.16
let rec f = function
    | (x, []) -> []
    | (x, y::ys) -> (x+y)::f(x-1,ys);;

let rec g = function
    | [] -> []
    | (x,y)::s -> (x,y)::(y,x)::g s;;

let rec h = function
    | [] -> []
    | x::xs -> x::(h xs)@[x];;

```
