> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Arrays.md
## Arrays
[https://wiki.haskell.org/Arrays#Mutable_arrays_in_ST_monad_.28module_Data.Array.ST.29](https://wiki.haskell.org/Arrays#Mutable_arrays_in_ST_monad_.28module_Data.Array.ST.29)

[https://hackage.haskell.org/package/array-0.5.0.0/docs/Data-Array-MArray.html](https://hackage.haskell.org/package/array-0.5.0.0/docs/Data-Array-MArray.html)


#### Mutable IO arrays (module Data.Array.IO)
The second interface is defined by the type class MArray (which stands for "mutable array" and is defined in the module Data.Array.MArray) and contains operations to update array elements in-place. Mutable arrays are very similar to IORefs, only they contain multiple values. Type constructors for mutable arrays are IOArray and IOUArray and operations which create, update and query these arrays all belong to the IO monad:

```
 import Data.Array.IO
 main = do arr <- newArray (1,10) 37 :: IO (IOArray Int Int)
           a <- readArray arr 1
           writeArray arr 1 64
           b <- readArray arr 1 
           print (a,b)
```

This program creates an array of 10 elements with all values initially set to 37. Then it reads the first element of the array. After that, the program modifies the first element of the array and then reads it again. The type declaration in the second line is necessary because our little program doesn't provide enough context to allow the compiler to determine the concrete type of `arr`. Unlike examples, real programs rarely need such declarations.


#### Mutable arrays in ST monad (module Data.Array.ST)
In the same way that IORef has its more general cousin STRef, IOArray has a more general version STArray (and similarly, IOUArray corresponds to STUArray). These array types allow one to work with mutable arrays in the ST monad:
```
 import Control.Monad.ST
 import Data.Array.ST

 buildPair = do arr <- newArray (1,10) 37 :: ST s (STArray s Int Int)
                a <- readArray arr 1
                writeArray arr 1 64
                b <- readArray arr 1
                return (a,b)

 main = print $ runST buildPair
```
Believe it or not, now you know all that is needed to use any array type. Unless you are interested in speed issues, just use Array, IOArray and STArray where appropriate. The following topics are almost exclusively about selecting the proper array type to make programs run faster.


```