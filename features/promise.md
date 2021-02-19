# Promise \(draft\)

### Introduction

To avoid the problems of coloured code, everything expression in KK can be Promise without looking like a Promise! This would means that there's no distinction between synchronous code and asynchronous code, which is a great improvements for users coming from JavaScript/TypeScript, so now you can focus on writing the plain old business logic in a concise way without asynchrony getting in your way.  

However, this also means that KK is not suitable for writing high performance algorithm, due to the overhead of Promises. Nonetheless, based on my experience, most of the business code is not about writing algorithms, but more of sanitising data and aggregating data, which are mostly business logic.

### Concurrency

Since everything is wrapped in Promise in KK, there's a syntax sugar for running multiple expressions in concurrent.

For example, say you want to fetch data from three different sources which are independent of each other, instead of writing like this:

{% code title="non\_concurrent.kk" %}
```typescript
let fetch_fruits = | null =>
  let Ok(bananas) = null.fetch_bananas()
  let Ok(apples) = null.fetch_apples()
  let Ok(grapes) = null.fetch_grapes()
  do !.notify_users()
  Ok({bananas, apples, grapes})
```
{% endcode %}

You can make them run concurrently using `&` 

{% code title="concurrent.kk" %}
```typescript
let fetch_fruits = | ! =>
    let Ok(bananas) = !.fetch_bananas()
    & let Ok(apples) = !.fetch_apples()
    & let Ok(grapes) = !.fetch_grapes()
    & do !.notify_users()
    Ok({bananas, apples, grapes,})
```
{% endcode %}

The difference between `non_concurrent.kk` and `concurrent.kk` is that in `non_concurrent.kk`, the fetching are done one by one, i.e. apples will only be fetched after bananas are fetched successfully.    
However in `concurrent.kk`, all kinds of fruits will be fetched at the same time.  

### Racing

To race between multiple values, i.e. the first one to fufill the pattern matching condition is resolved, we can use the `|` operator.  

For example,

```typescript
let eat_food = \null =>
  let Ok(food) 
    = null.get_rice()
    | null.get_pasta()
    | null.get_bread()
  Ok(food.eat())
```

