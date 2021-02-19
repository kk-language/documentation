# Idioms

### Function  with more than two arguments

Suppose we have the following function with 3 arguments:

```typescript
let replace = | s:String each:String with:String => 0
```

We should always refactor it into a 2 arguments function using [record](../features/record-object.md):

```typescript
let replace = | s:String {each:String new:String}) => 0
```

This is because we want to make the call site clear, for example, if we use the first `slice`, then it will look like this:

```typescript
do "Hello world".replace("Hello" "Bye")
```

The problem with the code above is that it's ambiguous, does it replace `"Hello"` with `"world"` or the other way around?  
Without checking the definition of `replace` , and for users without prior experience to the convention of function arguments placement in KK, they can only guess with little confidence.

Using record arguments, we can see that the problem above can be mitigated, for example with the second version of `slice` :

```typescript
do "Hello world".replace({each "Hello" with "Bye"})
```

Therefore, we should always refactor arguments with more than 2 arguments into a 2-argument function.

### Singleton enum

In some cases,  we might want to further differentiate a type into more specific types, for example, consider the following type:

```typescript
type People = {
  name:String
  phone:String
}
```

The type above has a problem, because it does not prevent us from assigning `name` to `phone` and vice versa. For example, the following code raises no compile error:

```typescript
let a: People = { name "Lee" phone "12345" }
let b: People = {
  name a.phone, // No error 
  phone "23456"
}
```

We can fix this with singleton enum:

```typescript
enum Name = Name(String)
enum Phone = Phone(String)
type People = { name:Name phone:Phone }

let a: People = {
  name Name("Lee")
  phone Phone("12345")
}

let b: People = {
  name a.phone // Error: type mismatch
  phone Phone("23456")
}
```

### Encapsulation

To achieve this in KK we have to use [singleton enum](idioms.md#singleton-enum) and [modules](../features/modules.md), with the following steps:

1. Create a singleton enum, don't export it 
2. Export the type alias of the singleton enum
3. Export the functions

Suppose we want to model the following Typescript class in KK:

```typescript
public class Vector {
  private elements: number[]
  public constructor(elements: number[]) {
    this.elements = elements
  }
  public at(index: number): number | undefined {
    this.elements[index]
  }
}
```

By, applying the rules above, we have:

```typescript
// Vector.kk
enum _Vector = _Vector({ elements: number[] })

export type Vector = _Vector

// Note that `new` is not a keyword in KK
// It's highlighted because we are using the Typescript syntax
// For writing the code snippet here
export let vector = | elements => _Vector({ elements })

// Note that we directly destructure Vector:New
export let at = | _Vector({elements,}) index =>
  elements.at(index)
```

And here's how we use it in another file:

```typescript
import "./" vector at

let myVector = [1].vector()

do myVector.at(0).print()  // Some(1)
```

