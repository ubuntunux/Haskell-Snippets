[https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html](https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html)<br>
[https://stackoverflow.com/questions/20558648/what-is-the-datakinds-extension-of-haskell](https://stackoverflow.com/questions/20558648/what-is-the-datakinds-extension-of-haskell)

Well let's start with the basics

### Kinds

Kinds are the types of types*, for example

```
Int :: *
Bool :: *
Maybe :: * -> *
```
Notice that -> is overloaded to mean "function" at the kind level too. So * is the kind of a normal Haskell type.

We can ask GHCi to print the kind of something with :k.

### Data Kinds

Now this not very useful, since we have no way to make our own kinds! With DataKinds, when we write

```
data Nat = S Nat | Z
```

GHC will promote this to create the corresponding kind Nat and

```
Prelude> :set -XDataKinds
Prelude> :k S
S :: Nat -> Nat
Prelude> :k Z
Z :: Nat
```

So DataKinds makes the kind system extensible.

### Uses

Let's do the prototypical kinds example using GADTs

```
{-# LANGUAGE DataKinds #-}
{-# LANGUAGE KindSignatures #-}
{-# LANGUAGE GADTs #-}

data Vec :: Nat -> * where
    Nil  :: Vec Z
    Cons :: Int -> Vec n -> Vec (S n)
```

Now we see that our Vec type is indexed by length.

That's the basic, 10k foot overview.

* This actually continues, Values : Types : Kinds : Sorts ... Some languages (Coq, Agda ..) support this infinite stack of universes, but Haskell lumps everything into one sort.