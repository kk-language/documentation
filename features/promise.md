# Promise \(draft\)

### Introduction

To avoid the problems of colored code, everything expression in KK is a Promise! This would means that there's no distinction between synchronous code and asynchronous code, which is a great improvements for users coming from JavaScript/TypeScript, so now you can focus on writing the plain old business logic in a concise way without asynchrony getting in your way.  

However, this also means that KK is not suitable for writing high performance algorithm, due to the overhead of Promises. Nonetheless, based on my experience, most of the business code is not about writing algorithms, but more of sanitizing data and aggregating data, which are mostly business logic.

### Concurrency

Since everything is wrapped in Promise in KK, there's a syntax sugar for running multiple expressions in concurrent.

For example, say you want to fetch data from three different sources which are independent of each other, instead of writing like this:

{% code title="non\_concurrent.kk" %}
```typescript
let fetch_fruits = \null =>
  let Ok(bananas) = null.fetch_bananas()
  let Ok(apples) = null.fetch_apples()
  let Ok(grapes) = null.fetch_grapes()
  Ok({bananas, apples, grapes})
```
{% endcode %}

You can write like this:

{% code title="concurrent.kk" %}
```typescript
let fetch_fruits = \null =>
  let Ok(bananas) = null.fetch_bananas(),
      Ok(apples) = null.fetch_apples(),
      Ok(grapes) = null.fetch_grapes()
  Ok({bananas, apples, grapes})
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

### Idea

Use the `let!` keyword.

```typescript
let getBomb = \null =>
  let! mentos = null.getMentos()
  let! coke = null.getCoke()
  mentos.putInto(coke)
```

The code above will be transpiled as:

```javascript
let getBomb = new Promise((resolve, reject) => {
  getMentos(null).then(mentos => 
    getCoke(null).then(coke => 
      putInto(mentos, coke).then(resolve)
    )
  )
  .catch(reject)
})
```

### Auto parallelization

In KK, promised will be run in parallel whenever possible, for example, `getMentos` and `getCoke` in the code below will run in parallel, because they did not depends on each other.

```typescript
let getBomb = \# => 
  let #resolved(mentos) = #.getMentos.await
  let #resolved(coke) = #.getCoke.await
  mentos.putInto(coke)
```

The code above is same as the following Javascript code:

```typescript
let getBomb = async () => {
  let [mentos, coke] = await Promise.all([
    getMentos(),
    getCoke()
  ])
  putInto(mentos, coke)
}
```

However, the following code will not be parallelized because the second promise depends on the first promise:

```typescript
let friedEgg = \# => 
  let #resolved(egg) = #.breakEgg.await
  let #resolved(friedEgg) = egg.fry.await
  friedEgg

```

### How to run promise in sequence?

In some cases, you might really want to run some sequence in even though the promises don't depends on each other.  
For example, we might think the following code works:

```typescript
let createBlock = \amount: number => 
  let #resolved(lock) = #.getLock.await
  let _ = amount.topup.await
  lock.release

let topup = async \amount: number => 
  // do something

```

It will not work as expected, because `getLock` and `topup` will be ran in parallel, since `topup` don't depends on the result of `getLock` .   
To fix this problem, we can add a ceremonial `lock` argument to `topup` :

```typescript
let createBlock = async \amount: number => 
  let #resolved(lock) = await #.getLock
  let #resolved(_) = await amount.topup(lock)
  lock.release


let topup = async \amount: number, _: Lock => 
  // do something

```

It might be tedious to write this kind of code, but there's actually benefits to it, for example, whoever want to call `topup` must get the lock first! And the compiler will complain if `lock` is not passed in to `topup`

{% hint style="info" %}
**Do you really want to run them in sequence?**  
Before you decide to use this approach, maybe you should think deeper why these promises have to run in sequences but still don't depends on each other. It might be a code smell.
{% endhint %}

### Race

This is same as `Promise.race`

```typescript
let #resolved(result) = promise1.race(promise2).await
```

### Promise creation

```text
#promise.new(\resolve, reject => ...)

123.promiseResolve
123.promiseReject
```

### Floating promises

Floating promises is not allowed in KK, every promises must be awaited.

