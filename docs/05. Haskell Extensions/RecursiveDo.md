[https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#recursivedo-and-dorec](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/guide-to-ghc-extensions/basic-syntax-extensions#recursivedo-and-dorec)<br>
[https://ocharles.org.uk/posts/2014-12-09-recursive-do.html](https://ocharles.org.uk/posts/2014-12-09-recursive-do.html)

DoRec으로 알려져있는 RecursiveDo는 모나딕 문맥안에서 재귀적인 값의 사용을 가능하게 해줍니다. 이것을 설명하기 위해서 let이 어떻게 동작하는지 살펴보자.
하스켈에서 let은 지연재귀(lazy recursion)가 가능합니다. 이렇게 작성해봅시다.
```
main = print $
    let x = fst y
        y = (3, x)
    in  snd y
```
y가 선언되기도 전에 x에서 y를 참조하고 있고 y에서도 재귀적으로 x를 참조하고 있습니다. 이것은 지연평가가 있기 때문에 가능한 것 입니다.  실행결과는 추측한대로 3입니다.
```
> main
3
```

기본적으로 do 표현식에서 이러한 지연 재귀는 불가능합니다. 하지만 MonadFix 타입 클래스는 이것을 가능하게 합니다. Control.Monad.Fix 모듈에 포함된 mfix가 이 일을 합니다. 
```
{-# LANGUAGE StandaloneDeriving #-}
import Control.Monad.Identity

main = print ((
    do y <- mfix $ \y0 -> do x <- return $ fst y0
                             y1 <- return (3, x)
                             return y1
       return $ snd y
    ) :: Identity Integer)
```
결과는 위와 마찬가지로 3입니다. 작동은 하지만 코드는 그리 예쁘지가 않습니다.
```
> main
Identity 3
```
</br>
이것을 좀더 보기좋게 바꾸기 위해 RecursiveDo 확장을 사용해보겠습니다. RecursiveDo는 mfix에 대한 신택틱 슈거로 mdo합수를 사용합니다.
```
{-# LANGUAGE StandaloneDeriving #-}
import Control.Monad.Identity

main = print ((
    mdo x <- return $ fst y
        y <- return (3, x)
        return $ snd y
    ) :: Identity Integer)
```
또한 rec함수가 있는데 이것 역시 mfix에 대한 신택틱 슈거입니다. mdo와 rec는 미묘하게 다릅니다.
rec는 mdo보다 좀더 "저수준"에 기반을 두고 있습니다. 아래는 rec를 사용한 예제입니다.
```
main = print ((
    do rec x <- return $ fst y
           y <- return (3, x)
       return $ snd y
    ) :: Identity Integer)
```

GHC가 let 바인딩을 만나면 모든것을 한번에 바인딩하는것이 아니라 의존성이 있는 그룹들로 분할하여 바인딩을 수행합니다.
```
let x = 1
    y = (x, z)
    z = fst y
    v = snd w
    w = (v, y)
in  (snd y, fst w)
```
실제로는 이런식으로 바인딩 된다고 보면된다.
```
let x = 1
in  let y = (x, z)
        z = fst y
    in  let v = snd w
            w = (v, y)
        in  (snd y, fst w)
```
let 바인딩 예제의 경우 코드는 다르지만 그 의미는 달라지지 않습니다. 하지만 모나드 문맥에서 이런식으로 코드를 분할하거나 그룹화하게 되면 그의미가 달라지게 될수 있습니다. mdo는 let 바인딩과 같이 코드분할을 수행하지만 rec는 그렇지 않습니다.
```
-- | expression 1 (equivalent to expression 2)
mdo x <- return 1
    y <- return $ (x, z)
    z <- return $ fst y
    v <- return $ snd w
    w <- return (v, y)
    return (snd y, fst w)

-- | expression 2 (equivalent to expression 1)
do x <- return 1
   rec y <- return $ (x, z)
       z <- return $ fst y
   rec v <- return $ snd w
       w <- return (v, y)
   return (snd y, fst w)

-- | expression 3 (not equivalent to expression 1 or expression 2)
do rec x <- return 1
       y <- return $ (x, z)
       z <- return $ fst y
       v <- return $ snd w
       w <- return (v, y)
   return (snd y, fst w)
```
결과는 모두 같습니다.
```
(1,(1,1))
```
</br>
expression 1 과 expression 2 는 mfix함수를 사용하여 대략적으로 아래와 같이 다시쓸수 있습니다.
```
do x <- return 1
   (y, z) <- mfix $ \(y0, z0) -> do y1 <- return $ (x, z0)
                                    z1 <- return $ fst y0
                                    return (y1, z1)
   (v, w) <- mfix $ \(v0, w0) -> do v1 <- return $ snd w0
                                    w1 <- return (v0, y)
                                    return (v1, w1)
   return (snd y, fst w)
```
반면에 expression 3 은 이렇게 변환될수 있습니다.
```
do (x, y, z, v, w) <- mfix $ \(x0, y0, z0, v0, w0) -> do x1 <- return 1
                                                         y1 <- return $ (x0, z0)
                                                         z1 <- return $ fst y0
                                                         v1 <- return $ snd w0
                                                         w1 <- return (v0, y0)
                                                         return (x1, y1, z1, v1, w1)
   return (snd y, fst w)
```
자! 사용해보자~!!
```
{-# LANGUAGE RecursiveDo #-}
import Control.Monad.State.Lazy

comp = do x0 <- get
          modify (+1)
          x1 <- get
          rec y <- return $ (x0, fst z)
              z <- return $ (x1, fst y)
          put 3
          return (y, z)

main = print $ runState comp 1
```
