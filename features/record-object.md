# Record / Object

Record if KK is very similar to Typescript object literal except:

* all properties are immutable
* no subtyping

For example:

```typescript
let people: {
  name: String
  job: {
    title: String
  }
} = {
  name "Lee"
  job {
    title "Engineer"
  }
}
```

### Record Punning

Instead of writing:

```typescript
let x = 1
let y = {x x}
```

We use pun the property name with comma:

```typescript
let x = 1
let y = {x,}
```

### Property Type Annotation

Since equals `=`  is used instead of colon `:`, we can easily annotate the type of a property, for example, say we want the `name` property of `people` to have type `String`:

```typescript
let people = {
  name: String "Lee"
  age 5
}
```

Because of this syntax decision, we can destructure an object with types without much boilerplate, unlike [in Typescript](https://github.com/microsoft/TypeScript/issues/29526).

```typescript
// Typecsript
let f = ({a, b: c}: {a: String, b: String}) => ...

// kk
let f = | {a: String b: String c} => ...
```

### No subtyping

In other words, no extra field is allowed. For example the following code will cause compile error:

```typescript
let ball 
  : {color: String} 
  = {
    color "Red"
    price 9
  } // Error, "price" field is extraneous 
```

### Updating a record

We can update a record using the `.{}` syntax:

```typescript
let people = {name "Lee", age 6}
let new_people = people.{ name "Wee" }
do people.log() // {name: "Lee", age: 6}
do new_people.log() // {name: "Wee", age: 6}
```

We can also update a value of a property based on its previous value. The syntax is as follows:

```c
expression '.' '{' property_name dot_expression '}'
```

{% hint style="info" %}
Dot expression means any of the following:

1. Record property access
2. Function invocation
3. Record update
{% endhint %}

For example:

```typescript
let a = {people {name "Bob" age 6}}

let b = a.{people.{name "John"}}

let c = a.{people.{age.plus(1)}}

do a.print() // {people {name "Bob" age 6}}
do b.print() // {people {name "John" age 6}}
do c.print() // {people {name "Bob" age 7}}
```

{% hint style="danger" %}
**Properties cannot be simply added**

In Typescript, the following code is valid:

```typescript
let people = {name: "Lee"}
let man = {
   ...people,
   age: 5
}
```

However in KK, the code above will be invalid because the properties of a record must be preserved.
{% endhint %}



### Optional properties and default value

KK does not allow optional fields, because it defeats type checking, most of the time when we want to added a new property for a type, the desirable behaviour is for the compiler to raise errors on all usage of the type, which is not possible with optional properties.



