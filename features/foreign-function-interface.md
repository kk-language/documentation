# Foreign Function Interface \(FFI\)

### Syntax

We can call Javascript function from KK by using `@@@` , for example:

```coffeescript
let plus
  : | Integer Integer => Integer 
  = @@@ (x, y) => x + y @@@
  
do 1.plus(2).print() # 3
```

### Considerations

Since KK does not support mutation, it is strongly recommended that users who would want to write Javascript wrappers to provide API that does not cause mutation.  
For example, let's consider the wrapper for `Array.push` method.  
This is a bad wrapper as it mutates the input:

```coffeescript
# Bad
let push<A>
  : | [A] A => [A]
  = @@@ (xs, x) => {x.push(x); return x} @@@
```

This is a good wrapper as it does not mutate the input:

```coffeescript
# Good
let push<A>
  : | [A] A => [A]
  = @@@ (xs, x) => [...xs, x] @@@
```

In general, [**referential transparency**](https://en.wikipedia.org/wiki/Referential_transparency) should be maintained.



### 

