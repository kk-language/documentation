---
description: >-
  This page describes the design goals of KK, which is used to guide the
  drafting of future feature proposals (i.e. RFC).
---

# Design goals of KK

## 1. Less is more \(Consistency\)

This is because power corrupts \(i.e. as less feature as possible\), which is also the core design goals of Go, Elm and Angular. This is crucial for maintaining consistency over a codebase, a great counter example is Haskell, due to the [vast amount of language extensions](https://wiki.haskell.org/Language_extensions), it's virtually impossible for any Haskeller to jump around different Haskell project and feels at home. The same problem applies to React, with conflicting features such as class vs functional components, even conflicting frameworks such as Flux vs Redux vs vanilla. 

In a corporate settings, code maintainability is one of the most important aspects, especially for huge projects where engineers are expected to be switched around to work on different features. 

Therefore, to see the usage of KK in a huge projects \(which is crucial for the continuous development of a language\), _less is more_ is set as the primary design goal of KK. 

However, this goal does not means that features cannot be added, but rather only really important features will be added. 

## 2. Fast compilation

One of the main motivation that pushed me into design and implementing this language is due the really slow compilation speed of TypeScript, which is a nightmare for me as a full-time TypeScript developer. 

Therefore, features that will dramatically slows down the overall compilation speed \(e.g. from `O(n)` to `O(n^2)` \) will be discouraged. Example of such features are multiple dispatch.

## 3. Safe, sound but flexible type system

This goals demand that all features in KK must not contain edge cases that cannot be resolved properly, or promotes the usage of creating holes in program to bypass type checking \(one example is the `any` type in TypeScript\). However KK does not take it to the extreme where even unsafe foreign function interfaces are banned \(notably Elm\). This is necessary to provide the user the confident of [_if it compiles, it works_](https://wiki.haskell.org/Why_Haskell_just_works), which is a very important user experience, such that the user will not treat the type system as an annoying nagger, but rather a helpful assistant \(which is my personal experience, in Java, I see the type system as an bureaucratic ceremony, because it's almost purposeless, as in the types does not guarantee that the program won't unexpectedly crashed. However in Haskell, it's a totally different behaviour, even though less type annotations are needed, but it really does guarantee that my program will only crash on my command\).

_Flexible_ means that type annotations should be less ceremonial as possible, one example is the anonymous struct type in TypeScript, on the contrary there's Rust, which required every struct to be defined up front, which can be very annoying for single-use struct.

## 4. Built-in promise/future mechanism

This goal is related to the first goal, which is to promote consistency. If there's no standard promise/future mechanism \(which is crucial for  concurrent programming\) supported from the beginning, the [problem of colored code](http://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/) will eventually appear. Colored code is a big problem as it prohibits the integration of libraries with different colors.  
Languages that suffers or suffered this problem are:

1. JavaScript: before the introduction of Promise, there are at least 3 types of popular mechanism, notably callbacks, Bluebird, pify and etc.
2. Rust: [async-std vs tokio](https://www.reddit.com/r/rust/comments/dngig6/tokio_vs_asyncstd/).
3. Purescript: [user-defined effects](https://github.com/purescript/purescript-effect).

Languages that don't have this problem are:

1. Go: where CSP \(communicating sequential process\) are supported from the beginning using keywords such as `go` , `defer` , `switch` , `case` etc.
2. Erlang/Elixir: where messaging is supported by default via the usage of bang `!` .

## 5. Built-in tools

Tools such as linter, formatter, testing primitives and language server should be supported by default to promote consistencies across different codebases. On top of that, all these tools should only allow minimal configuration. These are necessary to prevent unnecessary and unproductive arguments, for example in the Javascript community, users might fight over whether to use [Prettier](https://github.com/prettier/prettier) as the formatter or [Standard](https://standardjs.com/).  Go smartly avoided this problem by providing `gofmt` from the beginning. 

Another example is the build system of Haskell, where user can choose between Stack or Cabal, which might doubles the testing effort of library authors. Rust smartly avoided this problem by providing `cargo` from the beginning.  
  
This also solves the problem of configuration hell, usually in a Node.js project, you will have a lot of configuration files such as `eslintrc` , `prettierrc` , `webpack.config.js` , `package.json` etc and all of these make it hard to maintain a standard configurations over different projects, as they have to be copied all over. 

## 6. Strictly reproducible build

One of the nightmare as a Typescript developer is the non-consistent build of `npm`, which can cause a significant amount of engineering time to go into fixing package versions. This problem is worse n  React Native, because it depends not only on `npm` , but also `gradle` \(for Android build\) and `cocoapods` \(for iOS build\), and each of them allow [specifying version range](https://docs.npmjs.com/about-semantic-versioning#using-semantic-versioning-to-specify-update-types-your-package-can-accept), which is a feature that allows user to install a different version of the same package in the future without changing the package manifest file. Although this feature has its merits, for example it allows user to install hot patches implicitly, however it can cause build to be irreproducible, and also several problems:

1. Determining the suitable version of each package is NP-complete \(see [this article](https://research.swtch.com/version-sat)\)
2. Potential security hole \(see [npm event-stream incident](https://www.trendmicro.com/vinfo/dk/security/news/cybercrime-and-digital-threats/hacker-infects-node-js-package-to-steal-from-bitcoin-wallets), although not necessarily related\)

To prevent the issues above, KK should provide a mechanism for publishing and installing packages that will guarantee reproducible build.   
References:  
1. [https://www.lucidchart.com/techblog/2017/03/15/package-management-stop-using-version-ranges/](https://www.lucidchart.com/techblog/2017/03/15/package-management-stop-using-version-ranges/)

## 7. Railway programming

Since [railway programming](https://blog.logrocket.com/what-is-railway-oriented-programming/) is so ubiquitous, KK should support it in one form or another. The primary purpose of railway programming are:

1. Prevent unreadable nestings \(e.g. [callback hell](http://callbackhell.com/)\)
2. Reduce noises \(e.g. [Go's error handling](https://stackoverflow.com/questions/18771569/avoid-checking-if-error-is-nil-repetition)\)

Example of railway programming:

| Language | Feature | Railway Programming Mechanism |
| :--- | :--- | :--- |
| JavaScript  | Promise | [async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await) |
| C\# | Nullable | [null coalescing](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator)  |
| Haskell | Monad | [do notation](https://en.wikibooks.org/wiki/Haskell/do_Notation) |
| Rust | Result | [? operator](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html) |
| F\# | [Computation Expressions](https://docs.microsoft.com/en-us/dotnet/fsharp/language-reference/computation-expressions) | `let!` etc |

## 8. Familiarity

This language should be familiar to existing Typescript users, in other words, there should _no_ _unnecessary syntax difference_, all kind of syntaxes that differs from Typescript should have a strong justification, for example, disallowing some syntax because it's unsound 

