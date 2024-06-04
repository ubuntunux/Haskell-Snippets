[https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html](https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html)

[https://stackoverflow.com/questions/24481113/what-are-some-examples-of-type-level-programming](https://stackoverflow.com/questions/24481113/what-are-some-examples-of-type-level-programming)

[https://kseo.github.io/posts/2017-01-15-data-proxy.html](https://kseo.github.io/posts/2017-01-15-data-proxy.html)

```
{-# LANGUAGE DataKinds #-}
{-# LANGUAGE GADTs #-}
{-# LANGUAGE KindSignatures #-}
{-# LANGUAGE TypeFamilies #-}
{-# LANGUAGE UndecidableInstances #-}

data Nat = Zero | Succ Nat deriving Show

type family Add x y where
    Add 'Zero n = n
    Add ('Succ n) m = 'Succ (Add n m)    

data Vector (n :: Nat) (a :: *) where
    VNil :: Vector 'Zero a
    VCons :: a -> Vector n a -> Vector ('Succ n) a
    
instance Show a => Show (Vector n a) where
    show VNil         = "VNil"
    show (VCons a as) = "VCons " ++ show a ++ " (" ++ show as ++ ")"
    
append :: Vector n a -> Vector m a -> Vector (Add n m) a
append VNil xs           = xs
append (VCons a rest) xs = VCons a (append rest xs)
```
:kind 명령은 Add 타입 함수의 타입의 세부사항을 보여주고 :kind! 명령은 Add 타입 함수에 대한 평가결과를 보여준다.
```
λ> :kind Add (Succ (Succ Zero)) (Succ Zero)
Add (Succ (Succ Zero)) (Succ Zero) :: Nat

λ> :kind! Add (Succ (Succ Zero)) (Succ Zero)
Add (Succ (Succ Zero)) (Succ Zero) :: Nat
= 'Succ ('Succ ('Succ 'Zero))

λ> append (VCons 1 (VCons 3 VNil)) (VCons 2 VNil)
VCons 3 (VCons 1 (VCons 2 (VNil)))
```


다른예제)

```
{-# LANGUAGE DataKinds #-}
{-# LANGUAGE TypeFamilies #-}
{-# LANGUAGE TypeOperators #-}
{-# LANGUAGE UndecidableInstances #-}

import GHC.TypeLits
import Data.Proxy

-- value-level
odd :: Integer -> Bool
odd 0 = False
odd 1 = True
odd n = odd (n-2)

-- type-level
type family Odd (n :: Nat) :: Bool where
    Odd 0 = False
    Odd 1 = True
    Odd n = Odd (n - 2)

test1 = Proxy :: Proxy (Odd 10)
test2 = Proxy :: Proxy (Odd 11)
```

```
λ: :type test1
test1 :: Proxy 'False
λ: :type test2
test2 :: Proxy 'True
```

