# Modules \(WIP\)

## Import

The import syntax is as follows:

```c
IMPORT 
  = "import" URL NAME+
  
NAME
  = IDENTIFIER 
  | IDENTIFIER "=" IDENTIFIER
```

For example, suppose we have the following files, where name that ends with `/` represent a folder:

```text
root/
  main.kk
  Colors.kk
  util/
    date.kk
```

And their respective contents:

{% tabs %}
{% tab title="colors.kk" %}
```typescript
export let colors = ["red" "green" "blue"]
export let foo = 1
export enum Color = Red() Green()
export type People = {name: String age: Integer}

export let hello = |_:String => "i'm string"
export let hello = |_:Null => "i'm null"
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
    colors
    foo
    Color
    People
    hello
```
{% endcode %}

## Importing enums

Note that when an enum is exported, all of it's constructors are also exported, also when we import an enum, all of it's constructors will be brought into the current scope.   
For example, suppose we have a `Color` enum in `./color.kk` :

{% code title="./color.kk" %}
```typescript
enum Color = Red Green
```
{% endcode %}

When we import `Color` into `./main.kk` , then we can use the constructors already:

{% code title="./main.kk" %}
```typescript
import "./color.kk" Color

// Note that `Red` can be used here
do Red.print()
```
{% endcode %}

## Import aliasing

When we want to import two namespaces of the same name, we must alias one of them with a different name. For example \(arbitrary\):

```typescript
import "./util/date.kk" is_monday = is_monday_2
```

## Importing from Github \(WIP\)

Importing from Github is also possible, but we need to specify the commit hash or the tag name.   


{% hint style="info" %}
Why is commit hash or tag necessary?  
Refer [https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)
{% endhint %}

```typescript
import "https://github.com/kk/stdlib/tree/v0.0.1" 
    length,
    slice,
    map
```

## Re-export \(draft\)

We can import a namespace and export it, for example:

```typescript
from "./hello.kk" export foo
```

## Public Export \(Draft\)

This is for declaring value or function that will be consumed by the public, so that the KK compiler will not warn them as being unused.

This feature can also allow automatic semantic versioning, by looking into the source code history.

```typescript
public export x = 2
```

## Dead code elimination \(WIP\)

All unused declarations will be removed during transpilation.

