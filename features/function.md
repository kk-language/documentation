# Function

## Function syntax

The function syntax of KK is almost identical to that of Typescript.

#### Zero argument function

All function must take at least one argument in KK, so to emulate a  zero-argument function, you can use [null](types.md#null), which represent unit type in KK.

```typescript
let sayHello = (null) => "Hello world".print();
null.sayHello();
```

#### One argument function

```typescript
let square = (x: Integer): Integer => x.times(x);

2.square().print(); // 4
```

#### Two arguments function

```typescript
let multiply = (x: Integer, y: Integer): Integer =>
  x.times(y)

2.multiply(3).print(); // 6
```

#### Annotating function types

Type annotation are required for top level functions, however function that are declared in lower level need not.

```coffeescript
# Top level function requires type annotation
let square
  : | Float => Float
  = | x => x.multiply(2.0)
  
let main
  : | Null => Null
  = 
    # No need type annotation for `square`
    let square = | x => x.multiply(2.0)
    do 9.square.print
```

{% hint style="info" %}
Why is type annotation required for top level functions?  
This is because KK support single dispatch \(i.e. function overloading based on the type of the first parameter\). Therefore, without type annotation, KK will not be able to infer the type of each function properly.  
{% endhint %}

## Function call

In KK, function calling is done using the universal function call syntax \(UFCS\). Basically, functions are infix, the first argument comes before the function name.

For example, the following Typescript:

```typescript
print(join("Hello ", "World"))
```

Is the same as the following KK:

```typescript
"Hello ".join("world").print
```

Because of this reason, curried function will look very weird:

```typescript
let add = | x => | y => x.plus(y)
do 2.(3.add).print
```

{% hint style="info" %}
Should you curry in KK?  
_Note: this is a personal opinion_

From my personal experience, although currying make the code shorter, it reduces clarity, also it makes type error more cryptic, instead of getting compiler to tell you that you missed some arguments, you might receive some error saying you cannot some T to some U, which does not tell you at all about missing arguments.
{% endhint %}

> ### Generic functions

```typescript
let first<T>
  : | [T] => Option<T>
  = | xs => xs.at(0)
```

## How do invoke function properties in object?

```coffeescript
let math = {
  plus = | x y => x.plus(y)
}

# Wrong
let answer = math.plus(1 2)

# Correct (version 1)
let { plus } = math
let answer = 1.plus(2)

# Correct (version 2)
let answer = 1.(math.plus)(2)
```

## Dot shorthand

### Function Call

Instead of writing:

```typescript
| x => |x.value.add(1).minus(2)
```

We can write:

```typescript
|.value.add(1).minus(2)
```

### Record Property Access

Instead of writing:

```typescript
| x => x.foo.bar
```

We can write:

```typescript
|.foo.bar
```

### Record Property Update

Instead of writing:

```typescript
| x => x.{ y: 2 }
```

We can write:

```typescript
|.{ y: 2 }
```



## Optional Arguments

Optional function arguments is not supported in KK, however we can emulate it with [shorthand record property update](function.md#record-property-update).  
For example,

```coffeescript
type Preference = {
  quantity: Integer
  pay_by_cash: Boolean
}
let buy_food
  : | String | Preference => Preference => Null
  = | food_id update_preference => 
  let { quantity pay_by_cash } = {
    # Specify default values here
    quantity: 1
    pay_by_cash: true
  }.update_preference
  do [quantity pay_by_cash].print()
  null
  

# Usage
do "apple".buy_food(|.{}) # [1 true]
do "apple".buy_food(|.{quantity: 2}) # [2 true]
```

## Recursive functions

Functions in KK can be recursive by default. For example:

```typescript
let factorial
  : | Integer => Integer
  = | n =>
    if n.less_than(2)
      1
    n.minus(1).factorial.multiply(n)
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
  let x = 1.id
  let y = "Hello".id
  #       ^^^^^^^ Expected Integer, not String
```

#### 

