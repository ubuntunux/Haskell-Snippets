> [Haskell Snippets](../README.md) / [02. Haskell 기초](README.md) / Top level mutable state.md
## Top level mutable state
[https://wiki.haskell.org/Top_level_mutable_state](https://wiki.haskell.org/Top_level_mutable_state)

```
myGlobalVar :: IORef Int
{-# NOINLINE myGlobalVar #-}
myGlobalVar = unsafePerformIO (newIORef 17)
```