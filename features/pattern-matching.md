# Pattern Matching

Actually pattern match is just a functions in KK.

### Multiple variables

```typescript
Boolean = #True | #False

// syntax 1
and = {
  #True, #True => #True;
  _, _ => #False
}
[1,2,3].reduce(initial: 0, f: {
  acc, x => acc.plus(x)
})
result.{
  #Some(_) => "good",
  #None => "bad"
}


// syntax 2
and = {
  \#True, #True, #True
  \_, _, #False
}
[1,2,3].reduce(initial: 0, f: {
  \acc, x, acc.plus(x)
}
result.{
  \#Some(_), "good",
  \#None, "bad"
}

// syntax 2.1
let and: \boolean, boolean -> boolean = 
  \#True, #True => #True
  \_, _ => #False

let boom = 
  \x: number,
   y: number
  -> x.power(y)

[1,2,3].reduce(
  from: 0, 
  with: \acc, x => acc.plus(x)
)
result.(
  \#some(_) -> "good"
  \#none -> "bad"
)
[1,2,3].map(\x -> x.plus(2))

let orElse = <T>\x: Option<T>, defaultValue: T => 
  x.(
    \#some(x) -> x
    \#none -> defaultValue
  )

#none.orElse(2)

// syntax 3
let and 
 : (boolean, boolean) => boolean
 = (true, true) => true
 | (_, _) => false

[1,2,3].reduce(
  from = 0, 
  with = (acc, x) => acc.plus(x)
)
result.(
| #some(_) => {
    let result = "good"
  }
| #none => "bad"
)
isCrazy.(
  #true => "Crazy" | _ => "no"
)

// syntax 4
let and 
 : \boolean, boolean => boolean
 = 
 \true, true => true
 \_, _ => false

[1,2,3].reduce(initial: 0, f: 
  (acc, x) => acc.plus(x)
)
result.(
  \#some(_) => {
    let result = "good"
  }
  \#none => "bad"
)
isCrazy.(\true => "Crazy" \_ => "no")

// syntax 5
let and 
 : \(boolean, boolean) => boolean
 = 
 \(true, true) => true
 \(_, _) => false

[1,2,3].reduce(initial: 0, f: 
  \(acc, x) => acc.plus(x)
)
result.(
    \#some(_) => 
        let result = "good"
        result.concat(2)

    \#none => "bad"
)
isCrazy.(\true => "Crazy" \_ => "no")

// syntax 6
let and = 
  if (true, true) => true
  if (_, _) => false
[1,2,3].reduce(0, 
  if (acc, x) => acc.plus(x)
)
result.(
    if Some(_) => 
        let result = "good"
        result.concat(2)

    if None() => "bad"
)()
isCrazy.(if true => "Crazy" if _ => "no") 


// Other syntax
a
 > plus(2)
 > gt(3)
```

## Case exhaustiveness

Every cases of a union must be matched if the type is specified. If not it will be a compile error.

For example,

```typescript
enum Binary = Zero() One()

let foo =
  \(Zero(), Zero()) => true
  \(One(), Zero()) => false
  \(Zero(), One()) => false
//^ Non-exhausitve patterns. Missing:
//  \(One(), One())
```

## Any other cases

If you don't want to handle some cases, you can throw it into the `_` branch.  
For example:

```typescript
enum Color = Red() Green() Blue()
let is_red = 
  \Red() => true
  \_ => false
```

## Monadic bindings

### Example

Sometimes, a pattern match can get really nested, for example:

```typescript
let compute_bounds
  : \[Number] => Option<{lower: number, upper: number}>
  = \xs => 
  let xs = xs.sort()
  xs.first().match(
    \Some(lower) => xs.last.match(
      \Some(upper) => Some({lower, upper})
      \None() => None()
    )
    \None() => None()
  )
```

The code can get really unreadable with this kind of nesting, to improve the situation we can use monadic bindings:

```typescript
let computeBounds
  : \Array<Number> => Option<{lower: number, upper: number}>
  = \xs => 
  let xs = xs.sort()
  let Some(lower) = xs.first()
  let Some(upper) = xs.last()
  Some({lower, upper})
```

But in case you still want to handle the non-happy path, you still can do it with `else`:

```typescript
let computeBounds = \xs => 
    let xs = xs.sort()
    let Some(lower) = xs.first()
    let Ok(upper) = xs.last() 
        else \Error(_) => None() 
    Some({lower, upper})
```

### Single-case destructuring

If the destructure pattern we defined on the left is exhaustive, then there will be no implicit returns.

For example:

```typescript
enum Person = Person({name: String})

let name = \person =>
  let Person({name}) = person
  name
```

Suppose the pattern at line 4 \(of the code above\) is non-exhaustive, then there will be a compile error. For example:

```typescript
enum Person = Person({name: String})

let name = \person =>
  let Person({name = "Lee" }) = person 
  // Implicitly returning `person` if it does not match this pattern
  name
//^^^^ Error: Expected `Person`, got `String`
```

## Conditional pattern matching \(WIP\)

Since there's no if else statement in KK, how do we write the following JS code in KK?

```typescript
// Typescript
const grade = (score: number) => {
  if(score <= 40) {
    return "F"
  } else if(score < 60) {
    return "C"
  } else if(score < 80) {
    return "B"
  } else {
    return "A"
  }
}
```

```typescript
// kk
let grade = (
 \score if score.`<=`(40) => "F"
 \score if score.`<`(60) => "C"
 \score if score.`<`(80) => "B"
 \_ => "A"
)
```

## Pattern matchable types

### Enum

For example:

```typescript
enum Shape = 
    Square({side: Float})
    None

let area = 
    \Square({side}) => side.`^`(2.0)
    \None => 0.0

do Square({side= 3.0}).area().print() // 9.0
```

### Integer

For example:

```typescript
let is_zero = 
    \0 => true
    \_ => false
```

### Float

Floating point values cannot be pattern matched, because floating point values are not always equatable.

### Boolean

For example:

```typescript
let and = 
  \(true, true) => true
  \(_, _) => false
```

### String

For example:

```typescript
let is_zero_or_one = 
    \"zero" => true
    \"one" => true
    \_ => false
```

### Character

For example:

```typescript
let is_vowel = 
    \'a' => true
    \'e' => true
    \'i' => true
    \'o' => true
    \'u' => true
    \_ => false
```

### Null

Although null looks pointless for pattern matching, it actually simplifies the syntax for writing functions that accept null as argument. So instead of writing:

```typescript
let foo = \(_: Null) => true
```

We can write:

```typescript
let foo = \null => true
```

### Array

Unlike,Javascript, array pattern matching can only be one of the two cases below:

| Pattern | Meaning |
| :--- | :--- |
| `[]` | This array has exactly zero element. |
| `[x, ...xs]` | This array has at least one element. |

Therefore, the following valid pattern in Javascript is invalid in KK:

* `[x]`
* `[...xs]`
* `[a, b, ...xs]`

However, the KK version of pattern matching is more powerful, because both `x` and `xs` can also be a destructuring pattern.

For example, if we want to match an array with exactly two elements, we can use the following pattern:

```typescript
let [first, ...[second, ...[]]] = myArray
```

Array pattern matching in KK is pedantic \(this section is obsoleted\):

| Pattern | Meaning |
| :--- | :--- |
| `[]` | This array has zero elements. |
| `[...]` | This array has  at least zero elements. \(Basically this matches with any array\) |
| `[a]` | This array only has exactly one element. |
| `[a, ...]` | This array has at least one element, where `a` is the first element. |
| `[..., a]` | This array has at least one element, where `a` is the last element. |
| `[a, ..., b]` | This  array has  at  least two elements, where `a` is the first element and `b` is the last element. |
| `[...a, b]` | This array has  at least one element,  where `b` represents the last element while `a` represents all element before `b` \(can be empty array\). |
| `[a, b]` | This array has exactly two elements, where `a` is the first element and `b` is the second element. |
| `[a,...,b,...,c]` | Invalid pattern, cannot contain more than one spread \(triple dots\). |

### Record

For example:

```typescript
let is_origin = 
    \{ x = 0, y = 0 } => true
    \_ => false

do { x = 0, y = 0 }.print() // true 
do { x = 1, y = 2 }.print() // false
```

### Multiple function parameters

We can also pattern matching over multiple parameters of a function, for example:

```typescript
let and = 
  \(true, true) => true
  \(_, _) => false

do true.and(true).print() // true
do false.and(true).print() // false
do false.and(false).print() // false
```

## Nesting patterns

Patterns can be nested, for example:

```typescript
enum Color = Red() Blue()
type Animal = {
    color: Color,
    is_big: Boolean,
}

let is_red_big_animal = 
    \({color: Red(), is_big: true}: Animal) => true
    \_ => false
```

