---
description: This page will explain why KK will not support typeclass and traits.
---

# No typeclass/traits

Speaking from my personal experience \(less than a year\) with Haskell and Rust, I found that typeclass/traits has the following drawbacks:

1. It makes debugging or tracing really hard
   1. This is primarily because you need to reverse engineer which instance/implementation is being used,  which is a tedious and difficult task
2. It makes compile errors complicated, even Rust cannot describe this kind of errors in a nice way
3. It makes implementing Intellisense \(auto-complete\) exponentially harder
4. Since typeclass/traits essentially allows name overloading, and sometimes if go-to-definition don't work \(because of 3\), then it's essentially impossible to precisely locate the actual implementation of a given method/function without tedious trial-and-errors
5. It [breaks the codebase's consistency](https://medium.com/@eeue56/why-type-classes-arent-important-in-elm-yet-dd55be125c81) 
6. From 5, it makes it hard to read other's code, even though it's using the same language

And speaking from my experience as a full-stack Typescript developer for over a year, I generally finds that most of the code I've written do not requires the typeclass/traits mechanism, to be honest, most of them don't even requires parametric polymorphism \(a.k.a generics\).



