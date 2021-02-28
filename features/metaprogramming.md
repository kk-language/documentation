# Metaprogramming \(WIP\)

## Quotes

Expressions in KK can be quoted using backtick. Quoted expressions contain metadata such as line number, column number, representation and etc. 

For example:

```coffeescript
let print_line_number<T>
  : | Quoted<T> => Null
  = | x => x.meta.line_number.print()
  
do `"Hello"`.print_line_number() # 5
```



The meta programming of KK is purely based on [reflections](https://en.wikipedia.org/wiki/Reflective_programming). Unlike Rust or Lisp which is based on [marcos](https://en.wikipedia.org/wiki/Macro_%28computer_science%29) that allow users to create a syntactically different domain-specific language that can be embedded in the host language.

{% hint style="info" %}
Note, this has a problem, because the metaprogramming should be done at compile time, not run time.
{% endhint %}

  
Can be accessed via the `meta` property. For example,

```typescript
enum Color = Red() Green() Blue()

do Color.meta.constructors
  .map(.name.print())

// #red
// #green
// #blue
```

