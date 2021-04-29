# Statements

### Value definition

For example:

```typescript
let square = (n: Float): Float => n.multiply(n);
```

{% hint style="info" %}
No side effects

Top level value definitions cannot result in immediate function calls, this is to prevent unintended side effects when importing a module, and is also crucial for performing tree shaking properly. 

  
For example, the following is invalid:

```typescript
let my_value = "hello".concat("world")
//                     ^^^^^^ Cannot invoke function at top level
```
{% endhint %}

###  Type alias definition

```typescript
type Product<A, B> = {a: A, b: B};
```

### Enum definition

```typescript
enum Color = Red Green Blue
```

### Do statement

Syntax:

```c
do_statement = "do" expression
```

Semantics:  
1. The expression type of `do` statement must be `Null` 

2. `do` statements should only be compiled for entry point file

```typescript
do "Hello world".print()
```

