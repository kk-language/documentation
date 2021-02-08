# Best Pratices

### 1. Prefer Error over None

Due to the nature of KK, every unhandled variants will bubble up, therefore it is recommended that every function that may fail should return `Error` instead of `None` to **improve debuggability.**  
Say that we are defining a function \(`at`\) to get the item of an array at certain index.   
Suppose the user use this function as follows:

```typescript
let answer = [].at(0)
```

Instead of returning `None` , we should return `Error("index 0 is out of bound, the length of this array is 0")`

### 2. Use monadic bindings whenever possible

For example, read [monadic bindings.](../features/pattern-matching.md#monadic-bindings)

### 3. Do not abbreviate

Based on my observation and experience, abbreviations is very harmful for the readability \(and so maintainability\) of a codebase. Thus abbreviation should be avoided unless necessary. 

