[https://hackage.haskell.org/package/easytensor](https://hackage.haskell.org/package/easytensor)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-DataFrame-Type.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-DataFrame-Type.html)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Vector.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Vector.html)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Matrix.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Matrix.html)

[https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/MatrixFloatTest.hs](https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/MatrixFloatTest.hs)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Quaternion.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-Quaternion.html)

[https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/QuaterFloatTest.hs](https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/QuaterFloatTest.hs)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-DataFrame-SubSpace.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-DataFrame-SubSpace.html)

[https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-PrimBytes.html](https://hackage.haskell.org/package/easytensor-2.1.0.0/docs/Numeric-PrimBytes.html)

[https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/PrimBytesTest.hs](https://github.com/achirkin/easytensor/blob/v2/easytensor/test/Numeric/PrimBytesTest.hs)

[https://github.com/achirkin/easytensor](https://github.com/achirkin/easytensor)

The project consists of two parts:

- dimensions is a library to support type-level operations on lists of dimensions;
- easytensor wraps low-level operations on primitive byte arrays in a type-safe data type indexed over an element type and a list of dimensions.


```
> :set -XDataKinds
> :set -XGADTs
> :set -XTypeApplications
> :set -XTypeOperators 
> :set -XKindSignatures
> :set -XScopedTypeVariables

> import Numeric.DataFrame
> import Numeric.Dimensions

> 0::Scalar Int
S 0

> 0::Scf
S 0.0

> (vec2 1 0)::Vector Int 2
DF2 (S 1) (S 0)

> (DF2 1 0)::DataFrame Int '[2]
DF2 (S 1) (S 0)

>(vec2 1 0::Vector Double 2) ! (0:*U) :: Scalar Double
S 1.0

>index (1:*U) (vec2 1 0::Vector Double 2) :: Scalar Double
S 0.0

> :{
foo :: Vec3f -> Word -> Scf
foo v i = v ! (Idx i:*U)
:}
> foo (vec3 1 2 3) 2
S 3.0
> foo (vec3 1 2 3) 1
S 2.0


> (0::Vector Float 3)
DF3 (S 0.0) (S 0.0) (S 0.0)

λ> (0::Vector Float 3) + 9
DF3 (S 9.0) (S 9.0) (S 9.0)

> 3 :: DataFrame Float '[3]
DF3 (S 3.0) (S 3.0) (S 3.0)

> 3 :: DataFrame Float '[3,3]
DF3 (DF3 (S 3.0) (S 3.0) (S 3.0)) (DF3 (S 3.0) (S 3.0) (S 3.0)) (DF3 (S 3.0) (S 3.0) (S 3.0))

> (fromList [(vec2 1 0)::Vector Float 2]) :: DataFrame Float '[XN 0, N 2]
XFrame (DF1 (DF2 (S 1.0) (S 0.0)))

> [vec2 1 0, vec2 2 3, vec2 3 4, vec2 5 6]::[Vector Int 2]
[DF2 (S 1) (S 0),DF2 (S 2) (S 3),DF2 (S 3) (S 4),DF2 (S 5) (S 6)]

> :{
-- | Get number of points in a vector
dataFrameLength :: DataFrame t (xns :: [XNat]) -> Word
dataFrameLength (XFrame (_ :: DataFrame t ns)) = case dims @ns of
    n :* _ -> dimVal n
    U      -> 1
:}
> dataFrameLength (fromList [vec2 1 0, vec2 2 3, vec2 3 4, vec2 5 6]::DataFrame Int '[XN 0, N 2])
4

> update (2:*U) (scalar 777) ((vec3 1 2 3)::DataFrame Float '[3])
DF3 (S 1.0) (S 2.0) (S 777.0)

λ> updateSlice (1:*U) (99::Vector Float 2) (0::Vector Float 3)
DF3 (S 0.0) (S 99.0) (S 99.0)

> ewfoldl @_ @_ @'[3] (+) 10 $ vec3 1 2 3::DataFrame Float '[3]
DF3 (S 11.0) (S 12.0) (S 13.0)

> ewfoldr @Float @_ @'[3] (+) 1 (vec3 1 2 3::DataFrame Float '[3])
DF3 (S 2.0) (S 3.0) (S 4.0)

> ewfoldr @Float @'[3] (+) 1 (vec3 1 2 3::DataFrame Float '[3])
S 7.0

> iwgen @Float @'[] @'[4] (fromIntegral . fromEnum)
DF4 (S 0.0) (S 1.0) (S 2.0) (S 3.0)

>  iwgen @Float @'[] @'[2,1] (fromIntegral . fromEnum)
DF2 (DF1 (S 0.0)) (DF1 (S 0.0))

λ> packDF @Double @12 @'[3] 1 2 3 4 5 6 7 8 9 10 11 12
DF12 (DF3 (S 1.0) (S 1.0) (S 1.0)) (DF3 (S 2.0) (S 2.0) (S 2.0)) (DF3 (S 3.0) (S 3.0) (S 3.0)) (DF3 (S 4.0) (S 4.0) (S 4.0)) (DF3 (S 5.0) (S 5.0) (S 5.0)) (DF3 (S 6.0) (S 6.0) (S 6.0)) (DF3 (S 7.0) (S 7.0) (S 7.0)) (DF3 (S 8.0) (S 8.0) (S 8.0)) (DF3 (S 9.0) (S 9.0) (S 9.0)) (DF3 (S 10.0) (S 10.0) (S 10.0)) (DF3 (S 11.0) (S 11.0) (S 11.0)) (DF3 (S 12.0) (S 12.0) (S 12.0))

λ> bSizeOf $ packDF @Double @12 @'[3] 1 2 3 4 5 6 7 8 9 10 11 12 :: Int
288

λ> bAlignOf $ packDF @Double @12 @'[3] 1 2 3 4 5 6 7 8 9 10 11 12 :: Int
8

```

[https://github.com/achirkin/easytensor/issues/27](https://github.com/achirkin/easytensor/issues/27)
```
First of all, Vec3f is just a synonym for DataFrame Float '[3]. So you have here a nested DataFrame, normally, a preferred solution would be to have something like DataFrame Float '[XN k, N 3] (reads as "an array of 3D vectors, that has length of at least k"), or more general DataFrame Float (xns +: N 3). Though, you can always flatten nested DataFrames using joinDataFrame. But of course, you can surely use a nested DataFrame too.

Regarding your question, I don't remember if easytensor has this exact function, but you can make one using folds. For example, you can use sewfoldr, which reads as "simple element-wise right fold". That is, just like with a normal Foldable, you can write dataFrameToList = sewfoldr (:) [] to construct a lazy list (or sewfoldr (\x xs -> unScalar x : xs) [] for a nested DataFrame).
The Numeric.DataFrame.SubSpace module has a set of very flexible folds, maps, and traversals, which work with arbitrary dimensionalities (known or unknown at compile time). However, you may need to use lots of type annotations to fix the types (which makes it rather difficult use in the beginning).
```


easytensor/bench/misc.hs
```
{-# LANGUAGE DataKinds        #-}
{-# LANGUAGE GADTs            #-}
{-# LANGUAGE TypeApplications #-}
{-# LANGUAGE TypeOperators    #-}

import Numeric.DataFrame
import Numeric.Dimensions

import qualified Control.Monad.ST     as ST
import qualified Numeric.DataFrame.ST as ST

main :: IO ()
main = do
    putStrLn "Hello world!"
    print (D3 :* D2 :* U)

    print (fromList [vec2 1 0, vec2 2 3, vec2 3 4, vec2 5 6]
             :: DataFrame Int '[XN 0, N 2])
    print (fromList [vec4 1 0 2 11, vec4 2 22 3 0, vec4 3 4 0 0]
             :: DataFrame Double '[XN 0, N 4])
    print (fromList [vec2 0 0, vec2 2 22, vec2 2 22]
             :: DataFrame Float '[XN 0, N 2])
    print $ fromList [0 :: Scf, 1, 3, 5, 7]
    print ( fromList [9, 13, 2]
             :: DataFrame Float '[XN 0, N 5, N 2])
    print $ vec2 1 1 %* mat22 1 (vec2 2 (3 :: Float))

    -- Seems like I have to specify known dimension explicitly,
    -- because the inference process within the pattern match
    -- cannot escape the case expression.
    -- On the other hand, if I type wrong dimension it will throw a nice type-level error.
    () <- case (fromList [10, 100, 1000] :: DataFrame Double '[XN 0, N 2, N 4]) of
        XFrame m
          | KnownDims <- dims `inSpaceOf` m
              -- Amazing inference!
              -- m :: KnownNat k => DataFrame '[k,2,4]
            -> print $ m %* vec4 1 2.25 3 0.162
        _ -> print "Failed to construct a DataFrame!"
    putStrLn "Constructing larger matrices"
    let x :: DataFrame Double '[4,5,2]
        x =  DF4 (transpose $ DF2
                    (DF5  56707.4     73558.41    47950.074  83394.61    25611.629)
                    (DF5  53704.516  (-3277.478)  99479.92   18915.17    59666.938))
                 (transpose $ DF2
                    (DF5 (-3035.543)  15831.447   73256.625   80709.38   72695.04)
                    (DF5  50932.49    7865.496   (-4050.5957) 99839.41   10834.297))
                 (transpose $ DF2
                    (DF5  21961.227   29640.914   39657.19    81469.64   17815.506 )
                    (DF5 (-8484.239)  16877.531  65145.742    80219.67   81508.87  ))
                 (transpose $ DF2
                    (DF5  53105.71    16255.646   23324.957  (-4438.164) 35369.824 )
                    (DF5  67930.45    8950.834    64451.71    76685.57   6728.465  ))
        y :: DataFrame Double '[7,3]
        y = transpose $ DF3
          (DF7 70096.85  34332.492 3642.8867 25242.25  59776.234 12092.57 10708.498)
          (DF7 46447.965 37145.668 56899.656 85367.56  15872.262 87466.24 82506.76 )
          (DF7 50458.848 31650.453 71432.78  53073.203 59267.883 82369.89 78171.56 )
        z = ewgen x :: DataFrame Double '[7,3,4,5,2]
    print $ ewfoldl @_ @_ @'[2] (+) 10 z
    print $ ewfoldr @_ @_ @'[5,2] (+) 0 z + 10
    print $ ewfoldl (+) 10 z - ewfoldr @_ @_ @'[4,5,2] (+) 0 z - 10

    -- We can map arbitrary suffix dimension over the dataframe,
    -- indexing by prefix dimensions.
    -- At the same time, we can transform underlying element type
    --  or suffix dimensionality.
    -- For example, we can do tensor produt of every sub-tensor.
    putStrLn "\nConversions between element types and frame sizes."
    print $ iwmap @Int @'[7] @'[2,2] @_
                  (\(i:*U) v -> fromScalar . (scalar (fromIntegral i) +) . round
                                     $ vec3 0.02 (-0.01) 0.001 %* v
                  ) y

    -- Using elementWise function we can apply arbitrary applicative functors
    -- over subtensors.
    -- This means we can even do arbitrary IO for each subtensor
    -- indexed by suffix dimensions.
    putStrLn "\nWelement-wise IO!"
    rVec <- elementWise @Double @'[4] @_  @_
              (\v -> print v >> return (sqrt . trace $ v %* transpose v)) x
    putStrLn "\nTraces for each matrix element:"
    print rVec

    -- Updating existing frames
    print $ update (2:*U) (scalar 777) rVec
    print $ update (1:*3:*U) (vec2 999 555) x

    let matX = iwgen (scalar . fromEnum) :: DataFrame Int '[2,6,4]
        matY = iwgen (scalar . fromEnum) :: DataFrame Int '[5,4]
    putStrLn "Check carefully that this returns no garbage"
    print matX
    print (ewmap @_ @'[2,6] (`snocDF` scalar 111) matX :: DataFrame Int '[2,6,5])
    print matY
    print (ewmap fromScalar matY :: DataFrame Int '[5,4,3])

    -- Working with mutable frames
    print $ ST.runST $ do
      sdf <- ST.thawDataFrame matY
      ST.writeDataFrame sdf (0:*0:*U) 900101
      ST.writeDataFrame sdf (2:*2:*U) 900303
      ST.writeDataFrame sdf (4:*2:*U) 900503
      ST.unsafeFreezeDataFrame sdf

```

easytensor/bench/subspacefolds.hs
```
{-# LANGUAGE DataKinds        #-}
{-# LANGUAGE GADTs            #-}
{-# LANGUAGE TypeApplications #-}
{-# LANGUAGE TypeOperators    #-}

module Main (main) where

import Data.Maybe      (fromMaybe)
import Data.Time.Clock

import Numeric.DataFrame
import Numeric.Dimensions


type DList = [6,14,10,7,2,8,5]

main :: IO ()
main = do
    t0 <- getCurrentTime
    putStrLn $ "\nStarting benchmarks, current time is " ++ show t0
    let df = iwgen @Float @DList @'[] (fromIntegral . fromEnum)
    t1 <- df `seq` getCurrentTime
    seq t1 putStrLn $ "Created DataFrame, elapsed time is " ++ show (diffUTCTime t1 t0)

    putStrLn "\nRunning a ewfoldl on scalar elements..."
    let rezEwf = ewfoldl @Float @DList @'[] (\a x -> return $! fromMaybe x a + fromMaybe 0 a / (x+1)) (Just 1)  df
    t2 <- rezEwf `seq` getCurrentTime
    seq t2 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t2 t1)
    print rezEwf

    putStrLn "\nRunning a iwfoldl on scalar elements (not using idx)..."
    let rezIwf = iwfoldl @Float  @DList @'[] (\_ a x -> a +  a / (x+1)) 1 df
    t3 <- rezIwf `seq` getCurrentTime
    seq t3 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t3 t2)
    print rezIwf

    putStrLn "\nRunning a iwfoldr on scalar elements (using fromEnum idx)..."
    let rezIwf2 = iwfoldr @Float @DList @'[] (\i x a -> return $! fromMaybe 0 a + x / ((1+) . fromIntegral $ fromEnum i)) (Just 0) df
    t4 <- rezIwf2 `seq` getCurrentTime
    seq t4 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t4 t3)
    print rezIwf2

    putStrLn "\nRunning a iwfoldl on scalar elements (enforcing idx)..."
    let rezIwf3 = iwfoldl @Float @DList @'[] (\i a x -> i `seq` return $! fromMaybe 0 a + fromMaybe x a / (x+1)) (Just 1) df
    t5 <- rezIwf3 `seq` getCurrentTime
    seq t5 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t5 t4)
    print rezIwf3

    putStrLn "\nRunning a ewfoldl on vector4 elements..."
    let rezEwv1 = ewfoldl @Float @(Init DList) @'[Last DList]
                          (\a x -> return $! fromMaybe 2 a + fromMaybe 0 a / (1 + iwgen @_ @_ @'[] (\(Idx i:*U) -> x ! Idx (i+1) :* U )) )
                          (Just (3 :: DataFrame Float '[4])) df
    t6 <- rezEwv1 `seq` getCurrentTime
    seq t6 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t6 t5)
    print rezEwv1

    putStrLn "\nRunning a ewfoldr on vector3 elements..."
    let rezEwv2 = ewfoldr @Float @(Init DList) @'[Last DList]
                          (\x a -> return $! fromMaybe 2 a + fromMaybe 1 a / (1 + iwgen @_ @_ @'[] (\(Idx i:*U) -> x ! Idx (i+1):* U )))
                          (Just (3 :: DataFrame Float '[3])) df
    t7 <- rezEwv2 `seq` getCurrentTime
    seq t7 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t7 t6)
    print rezEwv2

    putStrLn "\nRunning a ewfoldr with matrix products..."
    let rezEwm = ewfoldr @Float @(Take 3 DList) @(Drop 3 DList)
                          (\x a ->  a + x %* (DF5 1 0.5 0.1 0.01 0.001)  )
                          (1 :: DataFrame Float (Init (Drop 3 DList) +: 3)) df
    t8 <- rezEwm `seq` getCurrentTime
    seq t8 putStrLn $ "Done; elapsed time = " ++ show (diffUTCTime t8 t7)
    print rezEwm




    putStrLn "Checking indexes"
    print $ df ! (2:*1:*1:*3:*1:*U :: Idxs (Take 5 DList))

```