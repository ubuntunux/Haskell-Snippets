[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-KindSignatures](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-KindSignatures)

[https://stackoverflow.com/questions/9857553/kind-signatures](https://stackoverflow.com/questions/9857553/kind-signatures)

The same way a type signature works for values, a kind signature works for types.
```
f :: Int -> Int -> Bool
f x y = x < y
```
Here, f takes two argument values and produces a result value. The equivalent for types could be:
```
data D a b = D a b
```
The type D takes two argument types and produces a result type (it is * -> * -> *). For example, D Int String is a type (which has kind *). The partial application D Int has kind * -> *, just the same way the partial application f 15 has type Int -> Bool.

So we could rewrite the above as:
```
{-# LANGUAGE KindSignatures #-}
data D :: * -> * -> * where
  D :: a -> b -> D a b
```
In GHCi, you can query types and kinds:
```
> :type f
f :: Int -> Int -> Bool
> :kind D
D :: * -> * -> *
```

Another Exames)
```
{-# Language GADTs #-}
{-# LANGUAGE KindSignatures #-}

data Term a :: * where
  Lit    :: a -> Term a
  Succ   :: Term Int -> Term Int
  IsZero :: Term Int -> Term Bool
  If     :: Term Bool -> Term a -> Term a -> Term a

data Vec :: * -> * -> * where
  Nil :: Vec n a
  Cons :: a -> Vec n a -> Vec n a

data Fix :: (* -> *) -> * where
  In :: f (Fix f) -> Fix f
```
