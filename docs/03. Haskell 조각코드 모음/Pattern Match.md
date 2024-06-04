```
f::Int -> Int -> IO Int
f x y 
  | 0 <- x, 0 <- y = return 9
  | otherwise = return 3

>>> f 0 0
9
>>> f 1 1
3
```