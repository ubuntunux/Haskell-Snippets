> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Scoped Type Variables.md
## Scoped Type Variables
[https://ocharles.org.uk/guest-posts/2014-12-20-scoped-type-variables.html](https://ocharles.org.uk/guest-posts/2014-12-20-scoped-type-variables.html)

[https://opentutorials.org/course/2063/11682](https://opentutorials.org/course/2063/11682)


아래 코드를 봅시다.
```
doublemap :: (a -> a -> b) -> [a] -> [b]
doublemap f xs = map mf xs where
  mf x = f x x
```

 f 함수는 a타입의 값 두 개를 받아 b 타입의 값을 리턴합니다. 그리고 xs 함수는 a 타입의 리스트죠. 이 때 f 함수를 xs 전체에 적용하는데, f 함수는 a 타입의 값을 2개 받으므로 xs 리스트의 원소를 순서대로 돌면서 같은 원소를 f 함수에 원자로 2개 넘겨서 적용해줍니다. 따라서 doublemap 함수는 아래와 같이 동작하게 됩니다.

```
ghci> doublemap (\x y -> x + y) [1,2,3,4]
[2,4,6,8]
ghci> doublemap (\x y -> show x ++ " and " ++ show y) [1,2,3,4]
["1 and 1", "2 and 2", "3 and 3", "4 and 4"]
```
여기서 중요한 건 코드 자체의 동작은 아닙니다. doublemap 함수를 위와 같이 작성해놓고, mf 함수의 타입이 뭔지 확실히 표시해주기 위해 where 절에서 mf의 타입을 명시해주면 어떻게 될까요?

```
doublemap :: (a -> a -> b) -> [a] -> [b]
doublemap f xs = map mf xs where
    mf :: a -> b
    mf x = f x x
```
이렇게 mf 함수의 타입을 명시해줄 경우 놀랍게도 컴파일 에러가 발생합니다. 이 때 컴파일 에러가 발생하는 중요한 이유는, doublemap의 타입을 나타낼 때 사용한 a,b라는 타입 변수와 mf의 타입을 나타낼 때 사용한 a,b가 같지 않다 라는 것입니다. Haskell에서 코드를 컴파일할 때 타입 변수의 적용 범위는 정확히 해당 타입 서명 내부까지입니다. 즉, where 절에서 나타내는 a,b와 doublemap의 코드에서 나타나는 a,b는 같은 것이 아닙니다.

 mf의 타입을 위와 같이 a -> b 라고 적어주었을 경우, 이 mf라는 함수는 모든 임의의 타입 a,b에 대해서 동작해야합니다. 즉, doublemap의 타입에서 나타난 특정한 타입 a,b에 대해서만 동작하는 것이 아니라는 뜻입니다. 그런데 mf의 구현에 사용된 함수 f는 모든 임의의 타입에 대해 동작하는 것이 아니라, doublemap의 타입 서명에서 나타난, 고정된 타입 a,b 에 대해서만 동작하는 함수죠. 이 때문에 컴파일 에러가 발생하는 것입니다(f의 타입은 doublemap이 호출되는 시점에서 특정한 타입 a,b에 대한 것으로 고정이 됩니다.)

 Haskell 컴파일러가 mf의 타입을 직접 명시해주지 않을 경우 알아서 타입을 잘 추론해주지만 위와 같이 타입을 명시할 경우 doublemap의 타입에서의 a,b와 mf의 타입에서의 a,b를 같은 것으로 취급하지 않기 때문에 컴파일 에러가 발생합니다. 코드가 이 예제처럼 단순한 경우 mf의 타입을 생략하는 것으로 해결될 수 있지만, 굉장히 복잡한 타입을 가진 함수의 경우 where 절 내에 있는 함수에 대해서도 타입을 적어주고 싶을 때가 많습니다. 이럴 때 사용할 수 있는 것이 ScopedTypeVariables 언어 확장입니다. 이 언어 확장을 사용하려면 소스 코드 맨 위에 {-# LANGUAGE ScopedTypeVariables #-}를 적어주어야 합니다.

```
{-# LANGUAGE ScopedTypeVariables #-}
 
doublemap :: forall a b. (a -> a -> b) -> [a] -> [b]
doublemap f = map mf where
  mf :: a -> b
  mf x = f x x
```

 이 확장을 써서 다음과 같이 코드를 작성할 경우 아무런 에러가 발생하지 않습니다. 이 확장을 사용할 경우, 해당 함수 스코프 내에서 사용되는 타입 변수를 forall을 이용해서 명시해주면 해당 타입 변수들은 where 절 내에서도 같은 이름인 경우 같은 타입 변수를 가리키는 것으로 인식합니다.