# Types

### Integer

`Integer`

Example values: `0` `-1` 

### Float

`Float`

Example values: `1.0` ,`-2.0`

### Character \(Not implemented yet\)

`Character` 

Example values: `'c'` `'1'` `'@'`

### String

`String`

Can be either double quoted or single quoted: `"Hello world"` 

### Boolean

`Boolean`

`true` or `false`

### Null

`Null` This is actually the [unit type](https://en.wikipedia.org/wiki/Unit_type#:~:text=In%20the%20area%20of%20mathematical,can%20be%20any%20singleton%20set.) for KK, in other words, `null`can only be assigned to type of `null` . Useful for creating function for initialising value and ending void functions. For example,

```typescript
// function to initialise values
let array = \null => []
let emptyArray = null.array()

// ending void function
let sayHelloWorld = \null =>
  do "Hello world".print()
  null
```

### Function

Example:

```typescript
\(left: Number, rigt: Number) -> Number
```

Arguments name can be omitted:

```typescript
\(Number, Number) -> Number
```

### Array

`[Number]`

### Record

Refer to [record](record-object.md).

### Enum

Refer to [variant](variants-union.md).

### Generics

A type can take type parameters, however, unlike TypeScript, every type parameters must be named, for example:

```typescript
type Box<Type> = {value: Type}

// Invalid
type NumberBox = Box<number>

// Valid
type NumberBox = Box<Type = number>
```

#### Generic functions

Unlike Typescript, KK only permits Rank-1 polymorphism, [higher ranked polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism#Higher-ranked_polymorphism) is not allowed to prevent abuse. In other words, KK only allow type variables to be declared at the topmost level. 

For example, the following TypeScript has no equivalent counterpart in KK:

```typescript
let foo = <T>(
  bar: <U>(u: U) => T
) => {
  // body
} 
```

In KK, type variables can only be defined before the `=` sign, for example:

```typescript
let identity<T> = \(x: T) -> T => x
```



