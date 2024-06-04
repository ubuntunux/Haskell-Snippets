> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Arrow.md
## Arrow
[https://ocharles.org.uk/guest-posts/2014-12-21-arrows.html](https://ocharles.org.uk/guest-posts/2014-12-21-arrows.html)

```
{-# LANGUAGE Arrows #-}

import Control.Monad.Identity (Identity, runIdentity)
import Control.Arrow (returnA)
```

For example, a basic Haskell expression
```
f :: Int -> (Int, Int)
f = \x ->
  let y  = 2 * x
      z1 = y + 3
      z2 = y - 5
  in (z1, z2)
  
-- ghci> f 10
-- (23, 15)
```

For example, Identity monad
```
fM :: Int -> Identity (Int, Int)
fM = \x -> do
  y  <- return (2 * x)
  z1 <- return (y + 3)
  z2 <- return (y - 5)
  return (z1, z2)

-- ghci> runIdentity (fM 10)
-- (23,15)
```

The let bindings in f become <- under do notation. Arrow notation supports a similar translation:
```
fA :: Int -> (Int, Int)
fA = proc x -> do
  y  <- (2 *) -< x
  z1 <- (+ 3) -< y
  z2 <- (subtract 5) -< y
  returnA -< (z1, z2)

-- ghci> fA 10
-- (23,15)
```

In arrow notation proc plays the part of “lambda”, i.e. backslash, \, <- plays the part of = and -< feeds an argument into a “function”. We use returnA instead of return.

```
--    y <- action1 -< x
--    z <- action2 -< y
```