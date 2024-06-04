```
fn :: Int -> Int -> String
fn x y = if x == 1 then "a"
         else if y <  2 then "b"
         else "c"
```

```
fn :: Int -> Int -> String
fn x y
  | x == 1 = "a"
  | y <  2 = "b"
  | otherwise = "c"
```

```
{-# LANGUAGE MultiWayIf #-}

fn :: Int -> Int -> String
fn x y = if | x == 1    -> "a"
            | y <  2    -> "b"
            | otherwise -> "c"
```