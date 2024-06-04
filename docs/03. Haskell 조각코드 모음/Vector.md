> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Vector.md
## Vector
[https://wiki.haskell.org/Numeric_Haskell:_A_Vector_Tutorial](https://wiki.haskell.org/Numeric_Haskell:_A_Vector_Tutorial)

[http://hackage.haskell.org/package/vector-0.5/docs/Data-Vector-Storable.html#t%3AVector](http://hackage.haskell.org/package/vector-0.5/docs/Data-Vector-Storable.html#t%3AVector)

[http://hackage.haskell.org/package/vector-0.12.0.3/docs/Data-Vector.html](http://hackage.haskell.org/package/vector-0.12.0.3/docs/Data-Vector.html)

[https://hackage.haskell.org/package/vector-0.12.1.2/docs/Data-Vector-Mutable.html](https://hackage.haskell.org/package/vector-0.12.1.2/docs/Data-Vector-Mutable.html)

[https://www.schoolofhaskell.com/user/commercial/content/vector](https://www.schoolofhaskell.com/user/commercial/content/vector)

[https://tech.fpcomplete.com/haskell/library/vector](https://tech.fpcomplete.com/haskell/library/vector)

example) Mutable Vector

```
-- This module demonstrates boxed vectors. That means they
-- contain values which are thunks, aka values of kind *, aka
-- values which may contain _|_. You can write any Haskell
-- value in here.
--
-- Optimizations:
--
-- 1) Use unsafeFreeze to avoid copying. See its haddocks.

-- 2) Use unsafeRead/unsafeWrite. These are, clearly, unsafe
-- operations. Use your discretion. And write tests! Or better yet,
-- use Liquid Haskell so that you have a proof of bounds checks.

module Main where
import           Control.Monad.ST
import qualified Data.Vector as V
import qualified Data.Vector.Mutable as MV
main :: IO ()
main = do
  -- Using the IO monad:
  vec <-
    do v <- MV.new 1
       MV.write v 0 (1 :: Int)
       V.freeze v
  print vec
  -- Using the ST monad:
  print
    (runST
       (do v <- MV.new 1
           MV.write v 0 (1 :: Int)
           V.freeze v))

{-
Output:
> main
[1]
[1]
-}
```


Mutable IOVector
```
{-# LANGUAGE TypeSynonymInstances    #-}
{-# LANGUAGE FlexibleInstances       #-}

import Control.Monad
import qualified Data.Vector as Vector
import qualified Data.Vector.Mutable as MVector

data GeometryBufferData = GeometryBufferData deriving (Show)
type GeometryBufferDataList = MVector.IOVector GeometryBufferData

instance Show GeometryBufferDataList where
    show a = "Vector[" ++ (show (MVector.length a)) ++ "]"
    
vectorTest :: [GeometryBufferData] -> IO GeometryBufferDataList
vectorTest geometryBufferDatas = do
    geometryBufferDataList <- MVector.new (length geometryBufferDatas)
    forM_ (zip [0..] geometryBufferDatas) $ \(index, geometryBufferData) ->
        MVector.write geometryBufferDataList index geometryBufferData
    return geometryBufferDataList
    
main = vectorTest [GeometryBufferData]
```