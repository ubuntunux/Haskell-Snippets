> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Pattern Match.md
## Pattern Match
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