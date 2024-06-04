```
{-# LANGUAGE MultiWayIf #-}

fn :: Int -> Int -> String
fn x y = if | x == 1    -> "a"
            | y <  2    -> "b"
            | otherwise -> "c"
```