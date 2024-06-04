Pattern Guard 왼쪽화살표(<-)를 사용하며 많은 인수에 대한 패턴일치가 필요할때 유용하다.

예제)
```
> f [x, y] | True <- x, True <- y = True
> f _                             = False
> f [True, True]
True
> f []
False
```

또 다른 예제)

```
data T a where
  T1 :: T Int
  T2 :: T Bool

f :: T a -> T b -> Bool
f c d | T1 <- c, T1 <- d = True
      | T2 <- c, T2 <- d = False
      | otherwise        = False
```

실행결과)

```
> f T1 T1
True
> f T2 T2
False
> f T1 T2
False
```