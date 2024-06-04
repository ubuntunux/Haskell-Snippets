> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Pattern Synonyms.md
## Pattern Synonyms
[https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms](https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms)

[https://mpickering.github.io/posts/2014-11-27-pain-free.html](https://mpickering.github.io/posts/2014-11-27-pain-free.html)

[https://ocharles.org.uk/posts/2014-12-03-pattern-synonyms.html](https://ocharles.org.uk/posts/2014-12-03-pattern-synonyms.html)

[https://downloads.haskell.org/~ghc/7.8.3/docs/html/users_guide/syntax-extns.html#pattern-synonyms](https://downloads.haskell.org/~ghc/7.8.3/docs/html/users_guide/syntax-extns.html#pattern-synonyms)

PatternSynonyms는 패턴 동의어를 만들수 있게 해준다. 

패턴 동의어는 단방향(Uni-directional), 양방향(bidirectional), 명시적 양방향(Explicitly-bidirectional), 연관(Associated), Typed pattern synonyms, Branching pattern-only synonyms, Record Pattern Synonyms, Associating synonyms with types 등으로 구분된다. 

#### Bidirectional pattern synonyms
선언된 패턴 동의어를 패턴매칭에 사용할 수 도 있고 직접 값으로 사용할 수 도 있다.
```
> :set -XPatternSynonyms
> pattern Nine = 9
> f Nine = "It's Nine."
> f 9
"It's Nine."
> Nine
9
```

#### Uni-directional pattern synonyms
단순히 패턴매칭에만 사용할 수 있고 생성자로 사용하지는 못한다. 아래처럼 Nine을 직접 평가하려 하면 예외가 발생한다.
```
> pattern Nine <- 9
> f Nine = "It's Nine."
"It's Nine."
> Nine
<interactive>:123:1: error:
    • non-bidirectional pattern synonym ‘Nine’ used in an expression
    • In the expression: Nine
      In an equation for ‘it’: it = Nine
```

#### Explicitly-bidirectional pattern synonyms
(id -> n) 이부분은 ViewPatterns 확장을 사용하였다. id함수는 단순히 입력을 반환한다.
명시적 양방향 패턴 동의어(Explicitly-bidirectional pattern synonyms)는 where를 사용하여 표현식을 패턴동의어 선언에 사용할수 있다. 또한 양방향이기 때문에 패턴매칭과 표현식에도 함수나 값처럼 사용할 수 있다.
```
pattern Succ :: Int -> Int
pattern Succ n <- (id -> n)
  where
    Succ n = n + 1
```
결과)
```
> f 2 = "Two"
> f (Succ 1)
"Two"
> g (Succ 1) = "One"
> g 1
"One"
```

#### Associated pattern synonyms
타입클래스안에 data types, type synonym 를 선언하는 것처럼 패턴동의어(pattern synonym)도 선언할 수 있다.
```
class ListLike l where
  pattern Nil :: l a
  pattern Cons :: a -> l a -> l a
  isNil :: l a -> Bool
  isNil Nil = True
  isNil (Cons _ _) = False
  append :: l a -> l a -> l a

instance ListLike [] where
  pattern Nil = []
  pattern Cons x xs = x:xs
  append = (++)

headOf :: (ListLike l) => l a -> Maybe a
headOf Nil = Nothing
headOf (Cons x _) = Just x
```

#### Typed pattern synonyms
So far patterns only had syntactic meaning. In comparison Ωmega has typed pattern synonyms, so they become first class values. For bidirectional pattern synonyms this seems to be the case
```
data Nat = Z | S Nat deriving Show
pattern Ess p = S p
```
And it works:
```
*Main> map S [Z, Z, S Z]
[S Z,S Z,S (S Z)]
*Main> map Ess [Z, Z, S Z]
[S Z,S Z,S (S Z)]
```

#### Branching pattern-only synonyms
*N.B. this is a speculative suggestion!
*
Sometimes you want to match against several summands of an ADT simultaneously. E.g. in a data type of potentially unbounded natural numbers:
```
data Nat = Zero | Succ Nat
type UNat = Maybe Nat -- Nothing meaning unbounded
```
Conceptually Nothing means infinite, so it makes sense to interpret it as a successor of something. We wish it to have a predecessor just like Just (Succ Zero)!

I suggest branching pattern synonyms for this purpose:
```
pattern S pred <- pred@Nothing | pred@(Just a <- Just (Succ a))
pattern Z = Just Zero
```
Here pred@(Just a <- Just (Succ a)) means that the pattern invocation S pred matches against Just (Succ a) and - if successful - binds Just a to pred.

This means we can syntactically address unbound naturals just like bounded ones:

```
greetTimes :: UNat -> String -> IO ()
greetTimes Z _ = return ()
greetTimes (S rest) message = putStrLn message >> greetTimes rest message
```
As a nice collateral win this proposal handles pattern Name name <- Person name workplace | Dog name vet too.

#### Record Pattern Synonyms
See [https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/record-pattern-synonyms](https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/record-pattern-synonyms)

#### Associating synonyms with types
See [https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/associating-synonyms](https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/associating-synonyms)

#### COMPLETE pragmas
See [https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/complete-sigs](https://gitlab.haskell.org/ghc/ghc/wikis/pattern-synonyms/complete-sigs)

#### Export Pattern Synonym
```
{-# LANGUAGE PatternSynonyms #-}

module Main
  ( pattern Nine ) where
  
pattern Nine :: Int
pattern Nine = 9
```

다른 예제)

```
Prelude> pattern DJust a b = (Just a, Just b)
Prelude> DJust 1 2
(Just 1,Just 2)
Prelude> DJust a b = (Just 3, Just 4)
Prelude> a
3
Prelude> b
4
```

또 다른 예제)
여기 간단한 타입이 하나 있다.
```
{-# LANGUAGE PatternSynonyms #-}

data Type = App String [Type]
```
Using this representations the arrow type looks like App "->" [t1, t2].
Here are functions that collect all argument types of nested arrows and recognize the Int type:
```
collectArgs :: Type -> [Type]
collectArgs (App "->" [t1, t2]) = t1 : collectArgs t2
collectArgs _ = []

isInt (App "Int" []) = True
isInt _ = False
```
Matching on App directly is both hard to read and error prone to write.
The proposal is to introduce a way to give patterns names:
```
-- Type signatures is optional
pattern Arrow :: Type -> Type -> Type
pattern Arrow t1 t2 = App "->" [t1, t2]
pattern Int = App "Int" []
```
And now we can write
```
collectArgs :: Type -> [Type]
collectArgs (Arrow t1 t2) = t1 : collectArgs t2
collectArgs _ = []

isInt Int = True
isInt _ = False
```
