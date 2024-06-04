> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / StandaloneDeriving.md
## StandaloneDeriving
GHC 확장기능들을 사용하다 보면 문법적으로 deriving을 사용할 수 없는 상황이 생긴다. 예를들어 일반적인 하스켈 코드를 사용하여 Show 타입클래스를 상속받아보자.

```
data Color = Red | Green | Blue deriving Show
```

아래는 StandaloneDeriving 확장을 사용한 예제이다.

```
{-# LANGUAGE StandaloneDeriving #-}

data Color = Red | Green | Blue deriving 

deriving instance Show Color
```