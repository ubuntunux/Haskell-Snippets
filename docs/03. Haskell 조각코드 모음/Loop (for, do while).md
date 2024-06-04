> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Loop (for, do while).md
## Loop (for, do while)
Simple for implementation.

```
> import Control.Monad
> import Data.Foldable

> -- simple loop
> replicateM_ 3 $ print "loop"
"loop"
"loop"
"loop"

> -- for loop
> for_ [1..4] print
1
2
3
4

> -- traverse_ is for_ with its arguments flipped.
> traverse_ print [1..4]
1
2
3
4

```

Simple do while implementation

```
import Control.Monad

let loop x = do
    print x
    when (x < 3) $ loop (x+1)
loop 0
```

Result)
```
0
1
2
3
```