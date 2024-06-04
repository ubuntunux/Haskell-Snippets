> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Unboxed types.md
## Unboxed types
[http://dev.stephendiehl.com/hask/#unboxed-types](http://dev.stephendiehl.com/hask/#unboxed-types)

[https://www.fpcomplete.com/blog/2015/02/primitive-haskell](https://www.fpcomplete.com/blog/2015/02/primitive-haskell)
[GHC.Prim](https://downloads.haskell.org/~ghc/8.2.2/docs/html/libraries/ghc-prim-0.5.1.1/GHC-Prim.html)

[MagicHash](https://downloads.haskell.org/~ghc/8.2.2/docs/html/users_guide/glasgow_exts.html#ghc-flag--XMagicHash)

[UnboxedTypes](https://downloads.haskell.org/~ghc/8.2.2/docs/html/users_guide/glasgow_exts.html#glasgow-unboxed)


Boxed or Unboxed

```
{-# LANGUAGE MagicHash #-}

import GHC.Exts

unboxed :: Int# -> Int# -> Int#
unboxed a# b# = a# +# b#

boxed :: Int -> Int -> Int
boxed (I# a#) (I# b#) = I# (unboxed a# b#)

Prelude> boxed 1 2
3
```

EXample)

```
{-# LANGUAGE BangPatterns, MagicHash, UnboxedTuples #-}

import GHC.Exts
import GHC.Prim

ex1 :: Int
ex1 = I# (gtChar# a# b#)
  where
    !(C# a#) = 'a'
    !(C# b#) = 'b'

ex2 :: Int
ex2 = I# (a# +# b#)
  where
    !(I# a#) = 1
    !(I# b#) = 2

ex3 :: Int
ex3 = (I# (1# +# 2# *# 3# +# 4#))

ex4 :: (Int, Int)
ex4 = (I# (dataToTag# False), I# (dataToTag# True))
```

Example)
```
{-# LANGUAGE MagicHash, UnboxedTuples #-}
{-# OPTIONS_GHC -O1 #-}

module Main where

import GHC.Exts
import GHC.Base
import Foreign

data Size = Size
  { ptrs  :: Int
  , nptrs :: Int
  , size  :: Int
  } deriving (Show)

unsafeSizeof :: a -> Size
unsafeSizeof a =
  case unpackClosure# a of
    (# x, ptrs, nptrs #) ->
      let header  = sizeOf (undefined :: Int)
          ptr_c   = I# (sizeofArray# ptrs)
          nptr_c  = I# (sizeofByteArray# nptrs) `div` sizeOf (undefined :: Word)
          payload = I# (sizeofArray# ptrs +# sizeofByteArray# nptrs)
          size    = header + payload
      in Size ptr_c nptr_c size

data A = A {-# UNPACK #-} !Int
data B = B Int

main :: IO ()
main = do
  print (unsafeSizeof (A 42))
  print (unsafeSizeof (B 42))
```

dataToTag example)
```
-- data Bool = False | True
-- False ~ 0
-- True  ~ 1

a :: (Int, Int)
a = (I# (dataToTag# False), I# (dataToTag# True))
-- (0, 1)

-- data Ordering = LT | EQ | GT
-- LT ~ 0
-- EQ ~ 1
-- GT ~ 2

b :: (Int, Int, Int)
b = (I# (dataToTag# LT), I# (dataToTag# EQ), I# (dataToTag# GT))
-- (0, 1, 2)

-- data Either a b = Left a | Right b
-- Left ~ 0
-- Right ~ 1

c :: (Int, Int)
c = (I# (dataToTag# (Left 0)), I# (dataToTag# (Right 1)))
-- (0, 1)

-- unpackCString# :: Addr# -> [Char]
print (unpackCString# "Hello World"#)
```