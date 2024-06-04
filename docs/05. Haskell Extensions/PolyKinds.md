> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / PolyKinds.md
## PolyKinds
[https://downloads.haskell.org/~ghc/7.8.4/docs/html/users_guide/kind-polymorphism.html](https://downloads.haskell.org/~ghc/7.8.4/docs/html/users_guide/kind-polymorphism.html)

Allow kind polymorphic types.

This section describes GHC’s kind system, as it appears in version 8.0 and beyond. The kind system as described here is always in effect, with or without extensions, although it is a conservative extension beyond standard Haskell. The extensions above simply enable syntax and tweak the inference algorithm to allow users to take advantage of the extra expressiveness of GHC’s kind system.

This extension enables kind-polymorphic types like the type-level identity function (which of course requires TypeFamilies):

```
{-# LANGUAGE PolyKinds #-}
{-# LANGUAGE TypeFamilies #-}

type family Id (a :: k) :: k where
  Id a = a
```
We can now apply Id to types of different kinds:
```
type IdInt   = Id Int    -- kind *
type IdMonad = Id Monad  -- kind (* -> *)
```

The type of f1 places more restrictions on its definition, while the type of f2 places more restrictions on its argument.

That is: the type of f1 requires its definition to be polymorphic in the kind k, while the type of f2 requires its argument to be polymorphic in the kind k.
```
f1 :: forall (k::BOX). (forall (a::k) (m::k->*). m a -> Int) -> Int
f2 :: (forall (k::BOX) (a::k) (m::k->*). m a -> Int) -> Int

-- Show restriction on *definition*
f1 g = g (Just True)  -- NOT OK. f1 must work for all k, but this assumes k is *
f2 g = g (Just True)  -- OK

-- Show restriction on *argument* (thanks to Ørjan)
x = undefined :: forall (a::*) (m::*->*). m a -> Int
f1 x  -- OK
f2 x  -- NOT OK. the argument for f2 must work for all k, but x only works for *
```