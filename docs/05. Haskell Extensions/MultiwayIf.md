> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / MultiwayIf.md
## MultiwayIf
```
{-# LANGUAGE MultiWayIf #-}

fn :: Int -> Int -> String
fn x y = if | x == 1    -> "a"
            | y <  2    -> "b"
            | otherwise -> "c"
```