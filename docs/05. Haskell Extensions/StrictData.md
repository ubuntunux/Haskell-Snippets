> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / StrictData.md
## StrictData
Strict 확장과 동일한 기능을 수행하지만 data 타입의 field에 대해서 명시적으로 게으른(lazyness)으로 선언할수 있도록 해줍니다.

StrictData 확장을 사용한 예제 입니다.

```
{-# LANGUAGE StrictData #-}

data LazyT = L ~Int
data StrictT = S Int

lazy :: LazyT -> Bool
lazy x = True

strict :: StrictT -> Bool
strict y = True
```

StrictData 확장을 사용하면  field의 타입앞에 ~를 명시적으로 붙여주어야만 게으른(lazyness) 타입이 됩니다. 반대로 S Int 타입은 엄격한(Strict) 타입으로 간주됩니다. 지연평가, 즉시평가가 어떻게 이루어지는지 테스트 해볼수 있도록 undefined 값을 입력해보자. 

```
data LazyT = L ~Int
data StrictT = S Int

lazy :: LazyT -> Bool
lazy x = True

strict :: StrictT -> Bool
strict y = True
```

지연평가 타입이라면 undefined가 평가되지 않을 것이기 때문에 예외가 발생하지 않을것이고 즉시평가에 의해 undefined가 평가된다면 예외가 발생될 것이다.

```
Prelude> lazy (L undefined)
True
Prelude> strict (S undefined)
*** Exception: Prelude.undefined
CallStack (from HasCallStack):
  error, called at libraries/base/GHC/Err.hs:78:14 in base:GHC.Err
  undefined, called at <interactive>:65:11 in interactive:Ghci17
```