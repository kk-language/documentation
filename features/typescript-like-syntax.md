# Typescript-like Syntax

### Grammar

```c
expression
  = block_expression
  | switch_expression
  | if_expression
  | record
  | function_call

block_expression =
  "{"
    (let_statement)+
    expression
  "}"

switch_expression = 
  "switch" expression "{"
    ("case" pattern ":" expression)+
  "}"

if_expression =
  "if" "(" expression ")"
    expression
  "else"
    expression
        
let_statement =
  "let" pattern "=" expression
```

### Example

```typescript
let factorial = (n: number): number => {
  if (n < 2) 
    1
  else 
    n * factorial(n - 1)
}
```

