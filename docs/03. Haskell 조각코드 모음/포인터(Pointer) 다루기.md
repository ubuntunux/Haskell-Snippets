[https://www.haskell.org/haddock/libraries/GHC.Ptr.html](https://www.haskell.org/haddock/libraries/GHC.Ptr.html)

[http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Ptr.html](http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Ptr.html)

[http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Utils.html](http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Utils.html)

[http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Alloc.html](http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Alloc.html)

[http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Array.html](http://hackage.haskell.org/package/base-4.12.0.0/docs/Foreign-Marshal-Array.html)

[https://stackoverflow.com/questions/2386210/how-to-get-a-pointer-value-in-haskell](https://stackoverflow.com/questions/2386210/how-to-get-a-pointer-value-in-haskell)

[https://www.seas.upenn.edu/~cis194/spring15/lectures/12-unsafe.html](https://www.seas.upenn.edu/~cis194/spring15/lectures/12-unsafe.html)

[https://ro-che.info/articles/2018-02-03-stableptr-undefined-behavior](https://ro-che.info/articles/2018-02-03-stableptr-undefined-behavior)

[https://wiki.haskell.org/Foreign_Function_Interface#Pointers](https://wiki.haskell.org/Foreign_Function_Interface#Pointers)

```
> import Foreign.Marshal.Utils
> import Foreign.Marshal.Alloc
> import Foreign.Storable
> x <- new (99::Int)
> peek x
99
> poke x 11
> peek x
11
> free x
```

```
import Foreign.Marshal.Utils
import Foreign.Marshal.Alloc
import Foreign.Marshal.Array
import Foreign.Storable

array :: [Int]
array = [1,2,3]

main = do
    arrayPtr <- newArray array -- Write a list of storable elements into a newly allocated
    pokeArray arrayPtr [4,5,6] -- Write the list elements consecutive into memory
    values <- peekArray 3 arrayPtr -- Convert an array of given length into a Haskell list.
    print values
    free arrayPtr
```

Result
```
>>> [4,5,6]
```


Stable Pointer

```
> import Foreign.StablePtr
> ptr <- newStablePtr 1
> print =<< deRefStablePtr ptr
1
> freeStablePtr ptr

```

Storable deriving

```
data MyStruct = MyStruct
    { d0 :: Double
    , d1 :: Double
    , d2 :: Double
    } deriving (Show)

instance Storable MyStruct where
    alignment _ = 8
    sizeOf _= 24
    peek ptr= MyStruct
        <$> peekByteOff ptr 0
        <*> peekByteOff ptr 8
        <*> peekByteOff ptr 16
    poke ptr (MyStruct d0 d1 d2) = do
        pokeByteOff ptr 0 d0
        pokeByteOff ptr 8 d1
        pokeByteOff ptr 16 d2
```