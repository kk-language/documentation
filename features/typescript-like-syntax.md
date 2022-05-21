# Grammar

### Grammar

```ebnf
expr
  = function
  | unary_function_call
  | binary_function_call

terminal_expr
  = string
  | number
  | alphabetic_identifier
  | symbolic_identifer
  | unit
  | let
  | object
  | array
  | parenthesized

unit
  = "(" ")"

parenthesized
  = "(" expr ")"

let
  = "(" let_item ("," let_item)* ")"
  
let_item
  = assignment
  | expr

assignment
  = pattern "=" expr

object
  = "{" (object_item ("," object_item)*)? "}"
  
object_item
  = keyed_object_item
  | auto_index_object_item (* used for emulating tuple *)

keyed_object_item
  = assignment
  
auto_index_object_item
  = "#" expr
  
unary_function_call
  = expr alphabetic_identifier
  
binary_function_call
  = expr alphabetic_identifier ":" terminal_expr
  | expr symbolic_identifier terminal_expr

function
  = function_branch ("|" function_branch)*

function_branch
  = pattern "->" expr
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
