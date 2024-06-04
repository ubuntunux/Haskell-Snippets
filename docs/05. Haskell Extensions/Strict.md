> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Strict.md
## Strict
[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-Strict](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-Strict)

하스켈은 기본적으로 데이타 타입을 게으른(lazyness) 지연평가 타입으로 간주하고 반대로 엄격한(strict) 즉시평가 타입을 사용하고 싶을때 ! 를 변수앞에 붙여주곤 합니다. ! 를 사용하려면 BangPatterns 확장을 활성화 해야 합니다.

Strict 확장을 사용하면 하스켈의 데이터 타입을 기본적으로 엄격한(strict) 즉시평가 타입으로 간주하게 해주고 게으른(lazyness) 지연평가 타입을 사용하고 싶으면 변수앞에 ~를 붙여주면 됩니다.

어떤 차이가 있는지 예를 들어 보겠습니다. 우선 BangPatterns을 사용한 일반적인 예제입니다.
```
{-# LANGUAGE BangPatterns -#}
f :: Bool -> Bool
f x = True

g :: Bool -> Bool
g !y = True
```
함수 f의 변수 x는 기본적으로 게으른(lazyness) 지연평가 타입으로 간주되기 때문에 undefined를 값으로 넘겨주어도 실제로는 평가할 필요가 없기 때문에 문제 없이 동작합니다. 하지만 g 함수의 경우는 !를 앞에 붙여 줌으로써 명시적으로 엄격한(strict) 즉시 평가 타입으로 선언되었기 때문에 undefined를 평가함과 동시에 예외를 발생시켜버립니다. 

```
Prelude> f undefined
True
Prelude> g undefined
*** Exception: Prelude.undefined
CallStack (from HasCallStack):
  error, called at libraries/base/GHC/Err.hs:78:14 in base:GHC.Err
  undefined, called at <interactive>:10:3 in interactive:Ghci3
```

이것과 동일하게 동작하는 예제를 Strict 확장을 이용하여 구현해 보겠습니다.

```
{-# LANGUAGE Strict -#}
f :: Bool -> Bool
f ~x = True

g :: Bool -> Bool
g y = True
```

함수 f 의 입력값인 x 앞에 ~을 붙여줌으로써 게으른(lazyness) 평가타입이 되었습니다. 반대로 g 함수의 입력값인 y의 앞에는 아무것도 없지만 엄격한(strict) 즉시평가 타입으로 간주됩니다. 실행결과 동일합니다.

```
Prelude> f undefined
True
Prelude> g undefined
*** Exception: Prelude.undefined
CallStack (from HasCallStack):
  error, called at libraries/base/GHC/Err.hs:78:14 in base:GHC.Err
  undefined, called at <interactive>:10:3 in interactive:Ghci3
```

추가적으로 Strict확장은 StrcitData확장을 포함합니다.