---
description: Also known as single dispatch.
---

# Function Overloading

## Function Overload / Single Dispatch

In KK, single static dispatch is supported based on the type of first parameter.

The following is valid:

```typescript
let foo = \(x: String) => "Hello"

let foo = \(x: Integer) => "World"

do "1".foo().print() // "Hello"
do 1.foo().print() // "World"
```

## Single Dispatch Disambiguation

When there is not enough type information for the compiler to decide which functions to use, we need to provide type annotation to allow disambiguation.

For example, the following results in compile error:

```typescript
let foo = \(x: String) => "Hello"
let foo = \(x: Boolean) => "World"

let bar = \x => x.foo()
                //^^^ Error: which `foo` do you mean?
```

In this case, we just need to annotate `x` to allow disambiguation:

```typescript
let bar = \(x: Boolean) => x.foo()

do true.bar().print() // "World"
```

## No specialization

In KK, specialization of single dispatch is disallowed, in other words, if a type is generic, it cannot be overloaded with concrete types and vice versa. For example:

```typescript
let foo = \(_: Float) => "Hello"

let foo<T> = \(_: T) => "World"
//  ^^^ Compile error: `T` overlaps with `Float`
```

Another example:

```typescript
let bar<T> = \(_: Option<T>) => null

let bar = \(_: Option<String>) => null
//  ^^^ Compile error: `Option<T>` overlaps with `Option<String>`
```

{% hint style="info" %}
### Why specialization is not allowed in KK?

First of all, specialization contains a lot of edge cases and messes up type checking \(e.g. look at [Rust RFC: impl specialization](https://github.com/rust-lang/rfcs/blob/master/text/1210-impl-specialization.md#hazard-interactions-with-type-checking)\).  
Secondly, enabling specialization can dramatically slows down type checking, which is against the second design goal.
{% endhint %}

## Conflict Resolution

To determine whether a new variants of a function conflicts with the previous variants, the following factor is taken into account to determine the identity of a variant:

1. The type of the first parameter

Note that other factors such as parameter name, type variable name, and etc are not taken into account to get the identity of a function.

For example, the following will result in compile error, although the two `foo` definition looks different:

```typescript
let foo = \(spam: String) -> Number => 123
let foo = \(bar: String) -> Number => 1
 // ^^^ Compile error: conflicting definition
```

The reason is because the type of the first parameter of both `foo` is `String`.

## Shadowing

Shadowing allows conflicting definitions, take the example above, if the second `foo` is placed in a child scope, then there will be no compile error:

```typescript
let foo = \(_: String) => "Hello"

do 
  let foo = \(_: String) => "World"
  "hello".foo().print() // "World"
```

## Cannot overload with non-function

If a name `X` does not have the type of function, then `X` cannot be overloaded with multiple definitions in the same scope and vice versa, for example, the following code is invalid:

```typescript
let foo = 1
let foo = \(x: String) => x
//  ^^^ Error: `foo` is already declared as a non-function
```

