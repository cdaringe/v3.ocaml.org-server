---
title: Compare the two methods of calculating Euler's totient function
number: "38"
difficulty: beginner
tags: [ "arithmetic" ]
---


```ocaml
let rec gcd a b = if b = 0 then a else gcd b (a mod b)

let coprime a b = gcd a b = 1

let phi n =
  let rec count_coprime acc d =
    if d < n then
      count_coprime (if coprime n d then acc + 1 else acc) (d + 1)
    else acc
  in
    if n = 1 then 1 else count_coprime 0 1

let factors n =
  let rec aux d n =
    if n = 1 then [] else
      if n mod d = 0 then
        match aux d (n / d) with
        | (h, n) :: t when h = d -> (h, n + 1) :: t
        | l -> (d, 1) :: l
      else aux (d + 1) n
  in
    aux 2 n

(* Naive power function. *)
let rec pow n p = if p < 1 then 1 else n * pow n (p - 1)

(* [factors] is defined in the previous question. *)
let phi_improved n =
  let rec aux acc = function
    | [] -> acc
    | (p, m) :: t -> aux ((p - 1) * pow p (m - 1) * acc) t
  in
    aux 1 (factors n)
```

# Solution

```ocaml
# (* Naive [timeit] function.  It requires the [Unix] module to be loaded. *)
  let timeit f a =
    let t0 = Unix.gettimeofday() in
      ignore (f a);
    let t1 = Unix.gettimeofday() in
      t1 -. t0;;
val timeit : ('a -> 'b) -> 'a -> float = <fun>
```

# Statement

Use the solutions of problems 
"[Calculate Euler&#39;s totient function φ(m)][totient]" and 
"[Calculate Euler&#39;s totient function φ(m) (improved)][totient-improved]" 
to compare the algorithms. Take the number of logical inferences as a measure for efficiency. Try to calculate φ(10090) as an example.

[totient-improved]: #CalculateEuler39stotientfunctionmimprovedmedium

```ocaml
timeit phi 10090
```
