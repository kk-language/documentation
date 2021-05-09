---
description: 'Also known as tagged union, sum types, variants, etc.'
---

# Enums

Enums in KK is also known as sum types or tagged union. For example, we can model shapes as follows:

```typescript
enum Shape =
  Circle({radius: number})
  Rectanlge({width: number, height: number})
  Nothing()
```

Then we can define a function that match over `Shape` , for example:

```typescript
let area = 
  \Circle({radius}) => radius.power(2).times(PI) 
  \Rectangle({width, height}) => width.times(height)
  \Nothing() => 0


do Circle({radius = 12}).area().print()
```

## Constructor Disambiguation

Sometimes, if we have constructors with the same name in different enums, we need to provide type annotations for the compile to disambiguate.

Suppose we have two enums:

```typescript
enum Food = Apple() Nut() Rice()
enum Tool = Bolt() Nut() Screw()
```

Using `Nut` directly will result in error.

```typescript
let x = Nut()
      //^^^ Error: which `Nut` do you mean?
```

To solve this, we just need to provide type annotations:

```typescript
let x: Food = Nut()
```

