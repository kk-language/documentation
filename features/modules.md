# Modules \(WIP\)

## Namespace

Before we talk about modules, we need to understand what is considered a namespace in KK:

1. Name of file that has the `kk` extension
2. Name of [enum](variants-union.md)

## Import

The import syntax is as follows:

```c
IMPORT 
  = "import"  URL [ALIASES]
  
ALIASES
  = "where" (ALIAS)+
  
ALIAS 
  = IDENTIFIER "=" IDENTIFIER
```

For example, suppose we have the following files, where name that ends with `/` represent a folder:

```text
root/
  cain.kk
  Colors.kk
  util/
    date.kk
```

And their respective contents:

{% tabs %}
{% tab title="colors.kk" %}
```typescript
export let colors = ["red", "green", "blue"]
export let foo = 1
```
{% endtab %}

{% tab title="date.kk" %}
```typescript
export let is_monday = .day_of_week().equals(Monday())
export let foo = "hello"
```
{% endtab %}
{% endtabs %}

The following are different ways on how we can import the exported members from `Colors.kk` and `Date.kk`.

{% code title="main.kk" %}
```typescript
import "./colors.kk"
import "./util/date.kk"
```
{% endcode %}

Note that when a file is imported, all of its exported member will be brought into the current scope.

### Clashing resolution

Use aliasing to resolve. tbc

{% code title="main.kk" %}
```typescript
import "./colors.kk" where 
   map = map2
   
import "./util/date.kk"
```
{% endcode %}



```typescript
import '.' Colors

//  We can use `colors` directly
do colors.print()

// Error
do Colors::colors.print() 
// ^^^^^^^^ Unnecessary namespace qualification
```

However, in case of name clashes, namespace qualification is necessary, for example:

```typescript
import '.' Colors
import '.' Date

do Colors::foo.print()
do Date::foo.print()
```

### Current  module qualification

When we defined a function F that has the same name with one of the imported member, and we want to use F in the current file, we can use the the filename to refer to the current module \(i.e. file\).

For instance \(continue from the example code above\):

{% code title="Test.kk" %}
```typescript
import "." Date

let foo = [1]

do Test::foo.print()
```
{% endcode %}

### Import aliasing

When we want to import two namespaces of the same name, we must alias one of them with a different name. For example \(arbitrary\):

```typescript
import "." Date
import "." Date as Date2
```

### Importing from Github

Importing from Github is also possible, but we need to specify the commit hash or the tagname:

```typescript
import "https://github.com/kk/stdlib/tree/v0.0.1" String
```

### Scope resolution operator precedence

The scope resolution operator \(`::`\) has higher precedence than dot operator \(`.`\).

Because of that, the following code is valid:

```typescript
do [1, 2, 3]
  .Array::map(.plus(2))
  .Array::filter(.`<`(2))
```

### Re-export

We can import a namespace and export it, for example:

```typescript
import "." export Foo
```

### Ambiguous Resolution

Suppose we have the following files:

{% code title="Foo.kk" %}
```typescript
export enum Color = Red()
```
{% endcode %}

{% code title="Bar.kk" %}
```typescript
export enum Color = Green()
```
{% endcode %}

Suppose the two files above are imported into `Main.kk` , and when we try to use the `Color` namespace, we will get an error:

{% code title="Main.kk" %}
```typescript
import "./" Foo, Bar
let x = Color::Red()
      //^^^^^ Error: Ambiguous namespace resolution
      //      Which one did you mean?
      //        Foo::Color
      //        Bar::Color
```
{% endcode %}

### Variable Shadowing

Since variable shadowing is allowed, so when referring to a non-top-level symbol \(variable, type, enum, etc\), it's not necessary to specify the namespace, for example:

```typescript
let foo = \x => x.plus(1)

let main = \_ =>
  let foo = \x => x.minus(1)
  do 2.foo().print()
     //^^^ this foo will refers to the `foo` at line 4
```

### Partial scope resolution

In KK, partial scope resolution is allowed, as long as the scoped namespaces can be identified uniquely. Suppose we have two variables `foo` that resides in two different namespaces:

```typescript
let foo1 = A::B::C::foo
let foo2 = D::B::F::foo
```

In order to disambiguate the `foo` from `A::B::C` and the `foo` from `D::B::F`, we can merely specify as such:

```typescript
// Non-ambiguous namespace resolution
// Valid
let foo1 = A::foo
let foo2 = D::foo

// Also valid
let foo1 = C::foo
let foo2 = F::foo

// Invalid, because `B` cannot be uniquely identified
let foo1 = B::C::foo
let foo2 = B::F::foo
```

### Public Export

This is for declaring value or function that will be consumed by the public, so that the KK compiler will not warn them as being unused.

This feature can also allow automatic semantic versioning, by looking into the source code history.

```typescript
public export x = 2
```

### Dead code elimination

All unused declarations will be removed during transpilation.

