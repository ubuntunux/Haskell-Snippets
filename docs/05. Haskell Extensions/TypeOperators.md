> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / TypeOperators.md
## TypeOperators
[https://typeclasses.com/ghc/type-operators](https://typeclasses.com/ghc/type-operators)
[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeOperators](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeOperators)
[https://ocharles.org.uk/posts/2014-12-08-type-operators.html](https://ocharles.org.uk/posts/2014-12-08-type-operators.html)

간단하게 말하면 (+), (-)등과 같은 기호들을 타입인것처럼 선언하는것을 가능하게 해준다. 

```
{-# LANGUAGE TypeOperators #-}
data a + b = Plus a b deriving Show
type Foo = Int + Bool
```
타입 표기에도 사용할수 있다.
```
> Plus 1 True :: Int + Bool
Plus 1 True
> Plus 1 True :: Foo
Plus 1 True
```

또다른 예제
```
-- | Sums: encode choice between constructors
infixr 5 :+:
data (:+:) (f :: k -> *) (g :: k -> *) (p :: k) = L1 (f p) | R1 (g p)
```
```
-- | Products: encode multiple arguments to constructors
infixr 6 :*:
data (:*:) (f :: k -> *) (g :: k -> *) (p :: k) = f p :*: g p
```