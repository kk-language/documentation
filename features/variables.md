# Variables

In KK, variables are immutable by default.

```typescript
let x = 2; 
```

### Scoped variables

Sometimes you want to create some temporary variables that is only relevant for a particular computation.   
For example, let say you have this code:

```typescript
let main = {
  let x = null.getX();
  let y = null.getY();
  let result = x.concat(y); // x and y not used after this line
  result.print();
};
```

If you want to hide `x` and `y` from the scope below, you can rewrite it as follows:

```typescript
let main = {
  let result = {
    let x = null.get_x();
    let y = null.get_y();
    x.concat(y);
  };
  // x and y cannot be accessed from here
  result.print();
};

```

{% hint style="info" %}
**Indentation does not matters**

The indentation for the code above is merely for readability purpose, you can chained them in one line if you wish. For example, the following example is valid:

```typescript
let main = { let x = a; let y = b; x.plus(y); }
```

However, it is recommended that you use the default formatter.
{% endhint %}

### Binding shadowing

Sometimes, we want to transform the type of a variable without changing its meaning:

```typescript
let main = {
  let input = console.read();
  let sanitized_input = input.sanitized();
  let parsed_input = input.parse();
  // ...
};
```

With variable shadowing, you don't have to keep renaming your variables like the code above:

```typescript
let main = {
  let input = console.read();
  let input = input.sanitize();
  let input = input.parse();
  // ...
};
```

But of course the example above is contrived, it could have been simply rewritten as :

```typescript
let main = {
  let input = console.read().sanitize().parse();
  // ...
};
```

{% hint style="info" %}
Binding shadowing does not work at top level, for example the following is invalid:

```typescript
let x = 1
let x = "hello"
//  ^ Error: duplicated variable
```
{% endhint %}

### Mutable references \(WIP\)

Even though variable in KK is immutable by default, it's still possible to have mutable references:

```typescript
let x = MutableCell(1)
do x.update(.add(1))
do x.value()
```

### Quoted identifier \(WIP\)

Identifier can be quoted with backtick, for example:

```typescript
let `@@` = 2
do `@@`.print() // 2
```

This is useful when we want to use some keyword as variable names, for example:

```typescript
let `let` = 1
do `let`.print() // 1
```

This is also useful for defining operators, for example:

```typescript
let `^` = \(_: Int, _: Int) => null
do 2.`^`(3).print() // null
```

