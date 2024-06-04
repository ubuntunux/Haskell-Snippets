> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Bang Pattern.md
## Bang Pattern
[https://ocharles.org.uk/posts/2014-12-05-bang-patterns.html](https://ocharles.org.uk/posts/2014-12-05-bang-patterns.html)

[https://opentutorials.org/course/2063/11209](https://opentutorials.org/course/2063/11209)

! (느낌표)를 변수앞에 붙이는 문법때문에 Bang!! BangPattern이라 이름이 붙은듯하다.

일반적으로 BangPattern을 사용하면 패턴 일치에 엄격해야 함을 나타냅니다. 이를 이해하려면 먼저 패턴 일치와 Haskell의 평가 전략 간의 상호 작용을 이해해야합니다. 함수로 넘어온 인자는 패턴 일치를 수행하기 전까지는 값이 평가되지 않습니다. 예를 들어, 다음과 같은 함수는 인수에 패턴 일치를 요구하지 않으므로 이에 대한 평가를 강요하지 않습니다.

```
hello :: Bool -> String
hello loud = "Hello."
```

입력값에 상관없이 결과는 항상 같으며 심지어 undefined이 입력으로 에러가 발생하지 않는다. 왜냐하면 인수를 평가하지 않았기 때문에 문제가 없는것이다.

```
-> hello True
"Hello."
-> hello False
"Hello."
-> hello undefined
"Hello."
```

그러나 아래와 같이 입력인수(Bool)에 대한 패턴매치를 수행하도록 하여 값에 대한 평가가 강제로 일어나도록 한다면 어떻게 될까?

```
hello2 :: Bool -> String
hello2 True = "Hello!"
hello2 False = "hello"
```

실행결과 undefined 를 입력할 경우 True인지 False인지 알수 없기 때문에 에러가 발생한다. 왜냐하면 패턴일치에 의해 undefined가 평가되었기 때문에 예외가 발생한 것이다.

```
-> hello2 True
"Hello!"
-> hello2 False
"hello"
-> hello2 undefined
*** Exception: Prelude.undefined
```

이렇게 입력 인수에 대해 강제적으로 평가를 하도록 하는것이 BangPattern이다. 
BangPattern을 이용한 구현은 아래와 같다. 인수앞에 !를 붙여주면 된다. 결과는 위와 마찬가지로 undefined을 입력했을때 예외가 발생한다.

```
{-# LANGUAGE BangPatterns #-}

hello3 :: Bool -> String
hello3 !loud = "Hello."
```

 BangPattern은 Haskell의 묵시적인 지연평가를 필요로 하지 않을때 유용하다. 지연평가는 매우큰 데이터 리스트를 다룰때 매우 많은 메모리를 필요로 하므로 결과적으로 퍼포먼스에 좋지 않은 영향을 줄수도 있다.
 이런 경우 오히려 지연평가를 하지 않음으로써 더 좋은 퍼포먼스를 내기도 한다.
 
BangPattern이 없다면 강제로 평가가 일어나도록 하기 위하여 아래와 같이 구현해야 한다.

```
mean :: [Double] -> Double
mean xs = s / fromIntegral l
  where
    (s, l) = foldl' step (0, 0) xs
    step (s, l) a = let s' = s + a
                        l' = l + 1
                    in s' `seq` l' `seq` (s', l')
```

BangPattern을 이용한다면 좀더 깔끔해진다.

```
mean :: [Double] -> Double
mean xs = s / fromIntegral l
  where
    (s, l) = foldl' step (0, 0) xs
    step (!s, !l) a = (s + a, l + 1)
```