> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / $ 연산자 - dollar operator.md
## $ 연산자 - dollar operator
```
$ 연산자는 보통 괄호 대신 쓰이곤 한다.

λ> :t ($)
($) :: (a -> b) -> a -> b
λ> putStrLn ("A" ++ "Z")
AZ
λ> putStrLn $ "A" ++ "Z"
AZ
```


```
$ 연산자는 중위 연산자 이므로 ($ "Z") 마치 (\f -> f "Z") 처럼 동작한다.

λ> map (\f -> f "Z") [('A':), ('B':)]
["AZ","BZ"]
λ> map ($ "Z") [('A':), ('B':)]
["AZ","BZ"]
```