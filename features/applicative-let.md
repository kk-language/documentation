# Applicative-Let

### Motivation

Sometimes, a pattern match can get really nested, for example:

```coffeescript
let compute_bounds
: | [Integer] => Option<{lower: Integer upper: Integer}>
= | xs => 
  let xs = xs.sort
  xs.first.match(
    | Some(lower) => xs.last.match(
      | Some(upper) => Some({lower upper})
      | None => None
    )
    | None => None
  )
```

The code can get really unreadable with  this kind of nesting, to improve the situation we can use applicative let \(which is a syntactic sugar\).   
To use this feature, we have to first declare a function that allow us to map the value of a container whenever possible:

```coffeescript
let map<A B>
: | Option<A> | A => Option<B> => Option<B>
= | Some(a) f => a.f
  | None _ => None
  
# examples
do Some(2).map(|.plus(2)).print # Some(4)
do None.map(|.plus(2)).print # None
```

With this function, we can rewrite the example above as follows:

```coffeescript
let compute_bounds
: | [Integer] => Option<{lower: Integer upper: Integer}>
= | xs => 
  let xs = xs.sort
  xs.first.map(|lower => 
    xs.last.map(|upper =>
      Some({lower upper})
    )
  )
```

Although it's much terser, but it is still very nested, and will potentially evolve into [Callback Hell](http://callbackhell.com/).    


This is where **applicative-let** comes in to play, the code below is semantically equivalent to the second example:

```typescript
let computeBounds =
: | [Integer] => Option<{lower:Integer upper:Integer}>
= | xs => 
  let xs = xs.sort
  let/map lower = xs.first
  let/map upper = xs.last
  Some({lower upper}) 
```

Basically the syntax for applicative-let is as follows:

```text
let/function a = x
y
```

And it is equivalent to:

```text
x.map(|a => y)
```

As you may guess, the `function` must be a binary function \(i.e. a two-parameter function\), and its second parameter must be type of function, otherwise a compile error will be thrown. 

{% hint style="info" %}
Side notes

If you have kept an eye on OCaml, you probably noticed that the syntax above is very similar to this recent [ppx\_let](https://github.com/janestreet/ppx_let) proposal. Weirdly, before I came up with this syntax, I did have a look on that proposal, but I did not understand it at that time, but after an inspiration that hit me at midnight, then when I looked back at the proposal, I find it shocking that it was actually almost equivalent to what I thought.
{% endhint %}

