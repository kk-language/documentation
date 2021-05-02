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

 

### Type alias definition

```typescript
type Product<A, B> = {a: A, b: B};
```

### Enum definition

```typescript
enum Color { Red, Green, Blue };
```

### Expression statement

Expressions can be written at top level like JavaScript, however, only the entry file's top-level expressions will be compiled.   
Suppose we have a file `A` importing file `B` and both of them has a top level expressions as follows:

```typescript
"Hello world".print();
```

When we execute `A` or `B` we will only see **one** line of `"Hello world"` instead of two.

