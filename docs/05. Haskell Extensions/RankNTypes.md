[https://ocharles.org.uk/guest-posts/2014-12-18-rank-n-types.html](https://ocharles.org.uk/guest-posts/2014-12-18-rank-n-types.html)

[https://opentutorials.org/course/2063/11681](https://opentutorials.org/course/2063/11681)

기본적으로 forall은 생략할수 있지만 명시적으로 forall을 사용하여 다음과 같이 정의할수있다. 우선 이런형식을 Rank 1-Type이라고 한다.
```
foo :: forall a b c. ([a] -> Int) -> [b] -> [c] -> Bool
foo f x y = f x == f y
```
서로 타입이.다른 리스트를 입력으로 받아서 길이를 비교하는 함수로 사용할수 있다. 예를들면 아래 예제는 True를 반환할것 같지만 에러이다.
```
foo length [1,2,3,4] ['a','b','c','d']
```
foo 함수가 위와 같이 호출이 될 경우 위의 a,b,c 타입은 각각 무엇으로 결정이 될까요? 대충 이야기하자면 b 타입은 Int, c 타입은 Char이 될 겁니다. 하지만 a 타입은 뭐라고 해야할까요? a는 Int 이면서 Char이어야하는데 a 라는 하나의 타입이 동시에 두 개의 타입을 나타낼 수는 없습니다. 이 문제는 전칭기호 forall이 타입서명의 맨 바깥쪽에 있기 때문에 생기는 문제입니다. 'a' 타입과 'b,c' 타입이 모두 같은 레벨에 있기 때문에 a가 두 개의 타입을 동시에 나타낼 수 없게 되는 거죠. 이 문제를 해결하려면 foo의 타입을 다음과 같이 적어주어야합니다.
```
foo :: forall b c. (forall a. [a] -> Int) -> [b] -> [c] -> Bool
```
b,c 타입보다 a 타입에 대한 전칭 기호가 한 단계 안쪽에서 정의되기 때문에 이 함수가 어떤 타입의 값에 적용되느냐에 따라 한 함수 내에서 a 타입이 Int가 될 수도, Char 타입이 될 수도 있는 겁니다. 
foo와 같이 최소 하나 이상의 rank-1 타입의 값을 인자로 받는 함수를 rank-2 함수라고 합니다. 이를 확장하면 rank-N 타입은 최소 하나 이상의 rank-(N-1) 타입의 값을 인자로 받는 함수라고 정의할 수 있지요.

Haskell에서 이런 타입을 사용하려면 {-# LANGUAGE Rank2Types #-} 또는 {-# LANGUAGE RankNTypes #-}를 소스 코드의 맨 위에 명시해주어야합니다.

 그렇다면 Rank-N 타입은 상당히 유용한 것인데 왜 언어에서 기본적으로 제공해주지 않는 걸까요? 가장 큰 이유는 바로 Rank-N 타입을 허용할 경우 함수의 타입을 결정불가능(undecidable)하게 되기 때문입니다. 즉, 프로그래머가 직접 함수의 타입을 명시해주어야만 합니다. Haskell은 모든 함수의 타입을 추론 가능하기를 원했고 그 때문에 Rank-N Type을 기본적으로 사용할 수 없는 타입 시스템을 사용합니다.