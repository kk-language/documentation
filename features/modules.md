# Modules \(WIP\)

## Namespace

Before we talk about modules, we need to understand what is considered a namespace in KK:

1. Name of file that has the `kk` extension
2. Name of [enum](variants-union.md)

## Import

The import syntax is as follows:

```c
IMPORT 
  = "from" URL "import" NAME ("," NAME)*
  
NAME
  = IDENTIFIER 
  | IDENTIFIER "as" IDENTIFIER
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
from "./colors.kk" import colors
from "./util/date.kk" import is_monday
```
{% endcode %}

Note that when a file is imported, all of its exported member will be brought into the current scope.

## Import aliasing

When we want to import two namespaces of the same name, we must alias one of them with a different name. For example \(arbitrary\):

```typescript
from "./util/date.kk" import is_monday as is_monday_2
```

## Importing from Github

Importing from Github is also possible, but we need to specify the commit hash or the tagname:

```typescript
from "https://github.com/kk/stdlib/tree/v0.0.1" 
  import String
```

## Re-export

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

