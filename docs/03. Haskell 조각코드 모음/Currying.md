[https://wiki.haskell.org/Currying](https://wiki.haskell.org/Currying)

커링(Currying)은 여러개의 파라미터를 튜플(tuple) 한개의 파라미터로 변환하는 과정이다.

```
f :: Num a => a -> a -> a
f  x y = x + y
```
함수 g는 함수 f를 커리화 한것이다.
```
g :: Num a => (a, a) -> a
g  (x, y) = x + y
```
Prelude에는 위와 같이 커링함수가 이미 준비되어 있다.

example) curry 함수는 다중 파라미터를 튜플로 변환하여  함수 g에 대입한다.
```
> curry g 1 2
3
```

example) uncurry 함수는 튜플을 다중 파라미터로 변환하여 함수 f에 대입한다.
```
> uncurry f (1, 2)
3
```