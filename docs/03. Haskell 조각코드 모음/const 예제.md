> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / const 예제.md
## const 예제
It's useful for passing to higher-order functions when you don't need all their flexibility. For example, the monadic sequence operator >> can be defined in terms of the monadic bind operator as
```
x >> y = x >>= const y
```
It's somewhat neater than using a lambda
```
x >> y = x >>= \_ -> y
```
and you can even use it point-free
```
(>>) = (. const) . (>>=)
```
although I don't particularly recommend that in this case.


A simple example for using const is Data.Functor.(<$). With this function you can say: I have here a functor with something boring in it, but instead I want to have that other interesting thing in it, without changing the shape of the functor. E.g.
```
import Data.Functor

42 <$ Just "boring"
--> Just 42

42 <$ Nothing
--> Nothing

"cool" <$ ["nonsense","stupid","uninteresting"]
--> ["cool","cool","cool"]
```
The definition is:
```
(<$) :: a -> f b -> f a
(<$) =  fmap . const
```
or written not as pointless:
```
cool <$ uncool =  fmap (const cool) uncool
```
You see how const is used here to "forget" about the input.