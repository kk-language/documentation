# Statements

### Value definition

All top level value definition requires type annotation. For example:

```typescript
let square
  : | Float => Float
  = | x => x.multiply(x)
```

It would be a syntax error if type annotation is not provided:

```coffeescript
let foo = 2
#       ^ Syntax error: expected `:` 
```

### Type alias definition

```typescript
type IntegerArray = [Integer]
```

### Enum definition

```typescript
enum Color = Red Green Blue
```

### Do statement

Do statements will only be executed in entry point file. 

```typescript
do "Hello world".print()
```

