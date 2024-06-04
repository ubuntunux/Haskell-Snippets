> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Reader, Writer, State.md
## Reader, Writer, State
[http://adit.io/posts/2013-06-10-three-useful-monads.html#the-writer-monad](http://adit.io/posts/2013-06-10-three-useful-monads.html#the-writer-monad)

[https://www.reddit.com/r/haskell/comments/7gl2wh/when_to_use_reader_vs_state/](https://www.reddit.com/r/haskell/comments/7gl2wh/when_to_use_reader_vs_state/)

```
import Control.Monad.Trans.Writer
import Control.Monad.Trans.Reader
import Control.Monad.Trans
import Control.Monad.State
```

writer example)
```
half :: Int -> Writer String Int
half x = do
        tell ("I just halved " ++ (show x) ++ "!")
        return (x `div` 2)
        
runWriter $ half 8
=> (4, "I just halved 8!")

runWriter $ half 8 >>= half
=> (2, "I just halved 8!I just halved 4!")
```

reader example)
```
greeter :: Reader String String
greeter = do
    name <- ask
    return ("hello, " ++ name ++ "!")
    
runReader greeter $ "adit"
=> "hello, adit!"
```

state example)
```
greeter :: State String String
greeter = do
    name <- get
    put "tintin"
    return ("hello, " ++ name ++ "!")


runState greeter $ "adit"
=> ("hello, adit!", "tintin")
```






First, using ReaderT:
```
flip runReaderT 1 $ do
    ask >>= liftIO . print
    local (+ 1) $ ask >>= liftIO . print
    ask >>= liftIO . print
```
This should print 1, 2, 1.
```
1
2
1
```


And now StateT:
```
flip runStateT 1 $ do
    get >>= liftIO . print
    modify (+1) >> (get >>= liftIO . print)
    get >>= liftIO . print
```
This prints 1, 2, 2.
```
1
2
2
((),2)
```