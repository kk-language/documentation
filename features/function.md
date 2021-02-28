# Function

## Function syntax

```c
Function =
BACKSLASH  
  FunctionParameters
  ["->" TypeAnnotation]
  "=>" Expression
  
FunctionParameters
  = Pattern
  | "(" (Pattern [":" TypeAnnotation])+ ")"
```

#### Zero argument function

All function must take at least one argument in KK, so to emulate a  zero-argument function, you can use [null](types.md#null), which represent unit type in KK.

```typescript
let say_hello = \null => "Hello world".print()
do null.say_hello()
```

Generally type annotation is not required in KK, because they can be inferred.

#### One argument function

```typescript
let square = \x => x.times(x)
do 2.square().print() // 4
```

#### Two arguments function

```typescript
let multiply = \(x, y) => x.times(y)
do 2.multiply(3).print() // 6
```

#### Annotating function types

You can annotate the arguments type as follows, although they are optional:

```typescript
let square = \(x: Float) -> Float => x.times(x)
```

## Function call

In KK, function calling is done using the universal function call syntax \(UFCS\). Basically, functions are infix, the first argument comes before the function name.

For example, the following Typescript:

```typescript
print(join("Hello ", "World"))
```

Is the same as the following KK:

```typescript
"Hello ".join("world").print()
```

Because of this reason, curried function will look very weird:

```typescript
let add = \x => \y => x.plus(y)
2.(3.add())()
```

{% hint style="info" %}
Should you curry in KK?  
_Note: this is a personal opinion_

From my personal experience, although currying make the code shorter, it reduces clarity, also it makes type error more cryptic, instead of getting compiler to tell you that you missed some arguments, you might receive some error saying you cannot some T to some U, which does not tell you at all about missing arguments.
{% endhint %}

> ### Generic functions

```typescript
let first<T> = \(xs: [T]) => 
  xs.at(0)
```

## How do invoke function properties in object?

```typescript
let math = {
  plus = \(x, y) => x.`+`(y)
}

// Wrong
let answer = math.plus(1, 2)

// Correct
let { plus } = math
let answer = 1.plus(2)
```

## Dot shorthand

### Function Call

Instead of writing:

```typescript
\x => \x.value.add(1).minus(2)
```

We can write:

```typescript
.value.add(1).minus(2)
```

### Record Property Access

Instead of writing:

```typescript
\x => x.foo.bar
```

We can write:

```typescript
.foo.bar
```

### Record Property Update

Instead of writing:

```typescript
\x => x.{ y = 2 }
```

We can write:

```typescript
.{ y = 2 }
```



## Optional Arguments

Optional function arguments is not supported in KK, however we can emulate it with [shorthand record property update](function.md#record-property-update).  
For example,

```typescript
type Preference = {
  quantity: Integer,
  pay_by_cash: Boolean,
}
let buy_food = \(
  food_id: String,
  update: \Preference -> Preference
) => 
  let { quantity, pay_by_cash } = {
    // Specify default values here
    quantity = 1,
    pay_by_cash = true
  }.update()
  do [quantity, pay_by_cash].print()
  null
  

// Usage
do "apple".buy_food(.{}) // [1, true]
do "apple".buy_food(.{quantity = 2}) // [2, true]
```

## Recursive functions

To define a recursive function, you must annotate the function name with a function type, for example:

```typescript
// This does not work, because of missing type annotation
let factorial 
  = \n =>
    let true = n.greaterThan(1) else \_ => 1
    n.multiply(n.minus(1).factorial())

// This works
let factorial
  : \number -> number // <- This annotation allows recursive function
  = \n =>
    let true = n.greaterThan(1) else \_ => 1
    n.multiply(n.minus(1).factorial())
```

## Generalisation

If you are familiar with Hindley-Milner type inference, you probably heard of generalisation, for example, the following function will be inferred to have the type of `forall a. a -> a`:

```coffeescript
let identity = | x => x
```

However, [since let should not be generalised](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tldi10-vytiniotis.pdf), KK does not perform generalisation.   
For example, the following program will be correct in a language that implement let-generalisation \(e.g. Haskell, OCaml\):

```haskell
-- Haskell
main =
  let id = \x -> x in
  let x = id 1
  let y = id "Hello"
  return ()
```

However, in KK, it will be invalid:

```coffeescript
let main = 
  let id = | x => x
  let x = 1.id()
  let y = "Hello".id()
  #       ^^^^^^^ Expected Integer, not String
```

#### 

