> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Existential Quantification.md
## Existential Quantification
[https://stackoverflow.com/questions/9259921/haskell-existential-quantification-in-detail](https://stackoverflow.com/questions/9259921/haskell-existential-quantification-in-detail)

[https://ocharles.org.uk/guest-posts/2014-12-19-existential-quantification.html](https://ocharles.org.uk/guest-posts/2014-12-19-existential-quantification.html)

[http://dev.stephendiehl.com/hask/#existential-quantification](http://dev.stephendiehl.com/hask/#existential-quantification)

An existentially quantified type allows you to ‘forget’ the type of a value, typically remembering only that the type belonged to some class. For example, we can define a type for any value that can be shown:

```
{-# LANGUAGE ExistentialQuantification #-}

data Showable = forall a. Show a => Showable a

intShowable :: Int -> Showable
intShowable n = Showable n

stringShowable :: String -> Showable
stringShowable s = Showable s

showShowable :: Showable -> String
showShowable (Showable x) = show x
```

As demonstrated by intShowable and stringShowable, a Showable can contain a value of any type, as long as that type implements the Show class. showShowable demonstrates that we can use this fact to show a Showable. Note that we can’t do anything else with a Showable: the existential type ‘forgets’ everything about the value contained in it except that it can be shown.

Existential types look like an obvious solution to various problems, but it often turns out that these problems are better solved in other ways. Jonathan Fischoff’s article lays out some alternatives to existential quantification.


또 다른 예제)

```
{-# LANGUAGE ExistentialQuantification #-}

data Box = forall a. Box a (a -> a) (a -> String)

boxa :: Box
boxa = Box 1 negate show

boxb :: Box
boxb = Box "foo" reverse show

apply :: Box -> String
apply (Box x f p) = p (f x)
```
실행결과)
```
Prelude> apply boxa
"-1"
Prelude> apply boxb
"\"oof\""
```

또다른 예제

```
{-# LANGUAGE ExistentialQuantification #-}

data SBox = forall a. Show a => SBox a

boxes :: [SBox]
boxes = [SBox (), SBox 2, SBox "foo"]

showBox :: SBox -> String
showBox (SBox a) = show a

main :: IO ()
main = mapM_ (putStrLn . showBox) boxes
-- ()
-- 2
-- "foo"
```