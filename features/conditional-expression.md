# Conditional Expression

Conditional expression can be written using the following syntax:

```c
conditional_expression 
  = "if" condition true_branch false_branch
```

For example:

```coffeescript
enum Grade = A B C Failed
let grade
  : | Float => Grade
  = | score =>
    if score.more_than(80.0) A
    if score.more_than(60.0) B
    if score.more_than(40.0) C 
    Failed
    
do 81.0.grade.print # A
do 19.0.grade.print # Failed
```

