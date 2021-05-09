# Foreign Function Interface \(FFI\)

_This section is not done yet._  
Unlike most languages, FFI in KK is type safe, drawing ideas from [Foreign Function Typing: Semantic Type Soundness for FFIs](https://wgt20.irif.fr/wgt20-final23-acmpaginated.pdf).  
The type safety is achieve via comparing the types in KK and that of the target language. This is possible because the target language is Typescript.

For example:

```typescript
let plus
  : \number, number => number 
  = ffi ":(x: number, y: number): number => x + y"
```

## Function

## Marco

Macro is for expanding expression directly into native Javascript without wrapping around a function, this is useful for defining builtin function.  
For example:

```typescript
// KK
let plus = @javascriptMacro(
    \(x: number, y: number) => number
    "_0 + _1"
)
let result = 1.plus(2).plus(3)

// Javascript
var result = (((1) + (2)) + (3))
```

