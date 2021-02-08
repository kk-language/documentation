# External data \(WIP\)

KK program has strong boundaries, in other words, external data that are parsed into KK program will be validated, this also implies that KK will generate some runtime code, unlike Typescript which perform full type erasures.  
KK in somewhere between Typescript and Elm, in Typescript external data is only casted unsafely; in Elm although it's safe, but it's very tedious to write decoders.    
In KK, decoders are generated automatically, therefore you can code like Typescript but have the same level of safety as Elm.  
For example:  


```typescript
let a = '{"x": 2}'.fromJSON<{x: number}>
// a is #Ok({x: 2})

let b = '"hello"'.fromJSON<number>
// b is #Error("Cannot parse string as number")
```

This is possible with the metaprogramming facilities of KK. For example:

```typescript
let fromJSON<T> = \(json: String): Result<
  Ok = T,
  Error = string
> =>
 T.meta.ast.match(
   \#string => json.fromJsonString()
   \#null => json.fromJsonNull()
   \#number => json.fromJsonNumber()
   \#record({keyTypePairs}) => ...
   ...
 )
```

