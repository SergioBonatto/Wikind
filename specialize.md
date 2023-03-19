Sometimes, we have, in context, the following situation:

- b   : ((x_2 : a) -> Type)
- b   = (m => (Either (Equal Nat n (Nat.double m)) (Equal Nat n (Nat.succ (Nat.double m)))))
...
- snd : (b fst)


Notice that the type of snd is (b fst), but b = (m => Either ...), so, by doing the reduction, we know that snd : Either .... As such, we could try pattern matching it, like:

match Either snd { ... }


But that will NOT work, because the type that Kind sees is (b fst), and not (Either ...). In languages like Agda, this equality is done automatically. Since Kind aims to be as fast as possible, it won't do certain "searches" at compile time, so you must do this manually using the "specialize" syntax:

let snd = specialize b into #0 in snd


This will tell Kind to substitute b for its first value (#0) in the type of snd. This will result in snd having the type Either ..., which we can match on. Note the #0 is there because a variable in the context can be equal to different expressions, so here we use the first one. This is necessary to prove certain theorems involving, for example, Sigma.

This info should be in our docs (including the guide, the feature list, and the book). Can someone update them?
