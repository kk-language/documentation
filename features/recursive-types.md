# Recursive Types

In KK, aliased type cannot be recursive, as it will result in infinite expansion, therefore, only nominal type can be recursive. 

{% hint style="info" %}
In  this context, nominal means _named_. 
{% endhint %}

The only nominal type that is definable in KK is [union type](variants-union.md).

For example, the following is invalid:

```typescript
type People = {
  name: String,
  son: People
}
```

However, the following type is valid:

```typescript
type People = #people({
  name: String
  son: People
})
```

