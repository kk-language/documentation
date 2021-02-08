# Effects

There is a very naive effect system in KK, it's way less powerful than that of say, Koka. In KK, it's merely a simple mark-and-check system, you cannot raise an effect and catch it somewhere else.

### How effects works?

If a function `f` is declared or inferred to have the effects `E` , then any function `g` that calls `f` will be inferred to have the effects of `E` .

Suppose `g` is declared to not have the effect `E`, then calling `f` within the `g` will result in compile error.

### Usage

To declare an effect:

```typescript
effect Console
```

To mark a function as having a certain effect:

```typescript
let acceptInvitation = \[
  // List of effects here
  Console,
  FileSystem,
  Database,
  PushNotification
](data: Data) => ...
```

To say that a function is free of effects:

```typescript
let pureFunction = \[](x: number): number => ...
```

### Effect inference

There are some built-in effects that will be inferred automatically:

#### 1. Partial

A function will have the `partial` effect if it's recursive.

