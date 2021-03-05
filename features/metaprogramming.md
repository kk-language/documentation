# Metaprogramming \(WIP\)

## Quoted expression

Expressions in KK can be quoted using backtick. Quoted expressions contain metadata such as line number, column number, representation and etc. 



For example:

```coffeescript
let print_line_number<T>
  : | `T` => Null
  = | x => x.meta.line_number.print()
  
do `"Hello"`.print_line_number() # 5
```

To ensure that user passed in a quoted value, we can use quoted types  \(which are also enclosed with backticks\), for example:

```coffeescript
let foo
  : `Integer` 
  = 123
  # ^^^ Error: expected `Integer`, but got Integer
```

### Purpose of quoted expression

The primary purpose of quoted expression is actually for implementing testing tools, where metadata are important to tell user where are the failed assertions. Without quoted expression, we might need to include behind-the-scene magic to achieve the same purpose.    
  
Also, besides testing, quoted expressions are also useful for tracing runtime errors, which is especially important in language that don't support throwing exceptions \(such as Rust and Haskell, and of course KK\), because sometimes more than one places can return the same error.    
For example, suppose we have two REST API endpoints, namely A and B, where both writes to a same file X. Suppose X is locked, we will see in the log that X is locked, but we will not know if it is caused by A or B, since there are no stack trace.   
Stack trace can be emulated using quoted string array, for example:

```coffeescript
let write_to
  : | String String => Result<Null String>
  = | content filename => # not implemented

let endpoint_a
  : | Request => Result<Null [`String`]>
  = | request =>
    let Ok(null) = request.content.write_to("X") else 
      | Error(errors) => Error(errors.push(`endpoint_a`))
    Ok(null)
```

### Quotes properties

A quoted expression with type `T` has the following record type:

```text
{
  value: T
  meta: {
    filename: String
    line_start: Integer
    line_end: Integer
    column_start: Integer
    column_end: Integer
  }
}
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

