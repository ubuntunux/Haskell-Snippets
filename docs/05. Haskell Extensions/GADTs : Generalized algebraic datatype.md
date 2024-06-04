[http://dev.stephendiehl.com/hask/#gadts](http://dev.stephendiehl.com/hask/#gadts)

[https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html](https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html)<br>
[Generalized Algebraic Data Types](https://www.youtube.com/watch?v=6snteFntvjM)

You may be wondering, “What does GADT stand for?” Richard Eisenberg will tell you that they’re [Generalized Algebraic Data Types](https://www.youtube.com/watch?v=6snteFntvjM), but that the terminology isn’t helpful, so just think of them as Gadts.

일반화된 대수적 자료형(Generalized algebraic datatype; GADT)은 값생성자의 타입을 명시적으로 쓸 수 있게 한다. 이것이 갖는 의미는 데이터 타입이 결정된다면 값생성자, 타입생성자의 타입도 결정되게 되다는 것입니다. 일반적인 data선언과 GADTs를 사용한 data선언을 비교해 봅시다. 아래 둘은 같은것입니다.
```
-- Vanilla
data List a
  = Empty
  | Cons a (List a)

-- GADTSyntax
data List a where
  Empty :: List a
  Cons :: a -> List a 
```

아래와 같이 Int, Bool 타입을 가질수 있는 Expr(표현식)이 있고 eval함수로 I Int, B Bool 두가지 타입을 모두 평가하는 다형성 함수를 만들어보자.

```
data Expr = I Int | B Bool

eval :: Expr -> Int
eval (I n) = n
```
I Int 타입에 대한 eval함수는 쉽게 만들수 있었지만 B Bool 타입에 대한 eval함수는 작성할수가 없다. 왜냐하면 B Bool값에 대한 함수의 타입 시그니쳐는 eval :: Expr -> Bool 이 되어야 하기 때문이다.

또 다른 시도를 해보자. Expr뒤에 오는 a타입변수는 선언은 되었지만 실제로는 사용되지 않는 팬텀타입이라고 부른다. 이렇게 팬텀타입을 사용하면 될것 같지만 a가 어떤 타입인지 알 수가 없기 때문에 에러가 발생한다.

```
data Expr a = I Int | B Bool

eval :: Expr a -> a
eval (I n) = n
eval (B b) = b
```

다음은 GADTs를 이용한 구현이다.
우선 ghci에서 GADTs 기능 켜기.

```
:set -XGADTs
```

GADTs 확장을 사용하면 생성자(constructor)에 대해 타입시그니쳐를 작성할수 있게 해준다. 생성자의 종류에 따라 타입변수 a의 타입이 결정된다는 것을 알 수 있다. I 생성자는 Expr Int를 반환하기 때문에 a는 Int가 되고 B 생성자는 Expr Bool을 반환하기 때문에 a는 Bool로 해석될 수 있다. 이제 eval함수는 Bool, Int 모두에 대해 사용할 수 있게 되었다.

```
{-# LANGUAGE GADTs #-}

data Expr a where
    I   :: Int  -> Expr Int
    B   :: Bool -> Expr Bool
    
eval :: Expr a -> a
eval (I n) = n
eval (B b) = b
```

실행결과)

```
>>> eval (I 9)
9
>>> eval (B True)
True
```
