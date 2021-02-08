# Statements

### Value definition

```typescript
let square = \x => x.multiply(x)
```

### Type alias definition

```typescript
type IntegerArray = [Integer]
```

### Enum definition

```typescript
enum Color = Red() Green() Blue()
```

### Do statement

Do statements will only be executed in entry point file. 

```typescript
do "Hello world".print()
```

### FFI

```typescript
ffi map<T, U> 
  : \(
    xs: T[],
    f: \T -> U
  ) -> [U] 
  = "(xs, f) => xs.map(f)"
```

