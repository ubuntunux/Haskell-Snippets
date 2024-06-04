> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / A Guide to Mutable References (IORef, MVar, TVar, TMVar, STRef).md
## A Guide to Mutable References (IORef, MVar, TVar, TMVar, STRef)
[https://www.kovach.me/posts/2017-06-22-mutable-references.html](https://www.kovach.me/posts/2017-06-22-mutable-references.html)

Performance :: STRef is faster than IORef

IORef, STRef examples
```
import Control.Monad.ST
import Data.IORef
import Data.STRef

exampleSTRef :: ST s Int
exampleSTRef = do
  counter <- newSTRef 0
  modifySTRef counter (+ 1)
  readSTRef counter

exampleIORef :: IO Int
exampleIORef = do
  counter <- newIORef 0
  modifyIORef counter (+ 1)
  readIORef counter
  
main :: IO ()
main = do
  v1 <- pure $ runST exampleSTRef  
  v2 <- exampleIORef
  print (v1, v2)
```