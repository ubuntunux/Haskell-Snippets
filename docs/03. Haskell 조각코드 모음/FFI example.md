[https://wiki.haskell.org/GHC/Using_the_FFI](https://wiki.haskell.org/GHC/Using_the_FFI)

[https://www.fpcomplete.com/blog/2015/05/inline-c/](https://www.fpcomplete.com/blog/2015/05/inline-c/)

```
// lib.c
#include "lib.h"
#include <stdio.h>
double getSize() {
    double size = 0;
    scanf("%lf", &size);
    return size;
}
```

```
// lib.h
double getSize(void);
```

```
-- Main.hs
{-# LANGUAGE ForeignFunctionInterface #-}
module Main where
import Foreign
import Foreign.C.Types
foreign import ccall "lib.h getSize" c_size :: IO Double

main :: IO ()
main = do a <- getLine
          b <- c_size
          print $ "got from C: " ++ show b
```

and you should compile it with:

```
$ ghc Main.hs lib.c
[1 of 1] Compiling Main             ( Main.hs, Main.o )
Linking Main ...
Then you can run it, supply a line for the Haskell getLine and a second line for the C scanf, and it should work fine:

$ ./Main
hello world!!   -- line for Haskell
135.0           -- line for C
"got from C: 135.0"
```