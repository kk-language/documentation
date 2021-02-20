# Modules \(WIP\)

## Import

### Syntax

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
export enum Color = Red Green
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

The following are different ways on how we can import the exported members from `colors.kk` and `date.kk`.

{% code title="main.kk" %}
```typescript
import "./colors.kk" {
    colors,
    foo,
    Color, 
    People,
    hello,
}
import "./date.kk" {
    is_monday,
}

```
{% endcode %}

### Importing enums

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

### Import alias

When we want to import two namespaces of the same name, we must alias one of them with a different name. For example \(arbitrary\):

```typescript
import "./util/date.kk" is_monday = is_monday_2
```

### Importing from remote \(WIP\)

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

### Re-export

KK does not allow re-export, as it can complicates the module structure of a project with unnecessary indirections.

## Encapsulation

Encapsulation can be achieved in KK via three level of access:

| Level | Example | Visible to current file | Can be imported via relative path | Can be imported via URL |
| :--- | :--- | :--- | :--- | :--- |
| Private | `private let foo = 1` | Yes | No | No |
| Protected | `let foo = 1` | Yes | Yes | No |
| Public | `public let foo = 1` | Yes | Yes | Yes |

### Protected \(Default\)

_Protected_ is the default level of access, because based on my experience, most symbols are usually shared with each files within a project. __However, protected symbols cannot be imported via absolute path, this prevents library users from importing symbols that the author did not intend to expose.

For example,

```typescript
let foo = 1
```

### Private

_Private_ restrict the access of a symbol to its own file, this is useful to prevent internal functions to be used by other files.  

For example,

```typescript
private let foo = 1
```

### Public

_Public_  is meant for exposing functions of a library that are meant to be consumed by other developers. Also, the compiler will not warn them as being unused.

This feature can also allow automatic semantic versioning, by looking into the source code history.

```typescript
public let x = 2
```

## Dead code elimination \(WIP\)

All unused declarations will be removed during transpilation.

