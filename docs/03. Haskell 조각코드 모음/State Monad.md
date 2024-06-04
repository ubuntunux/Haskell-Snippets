[http://hackage.haskell.org/package/mtl-2.2.2/docs/Control-Monad-State-Lazy.html](http://hackage.haskell.org/package/mtl-2.2.2/docs/Control-Monad-State-Lazy.html)<br>
[https://wiki.haskell.org/State_Monad](https://wiki.haskell.org/State_Monad)<br>
[https://gist.github.com/sdiehl/8d991a718f7a9c80f54b](https://gist.github.com/sdiehl/8d991a718f7a9c80f54b)

State 타입의 구조
```
-- s : type of state
-- a : type of value
newtype State s a = State { runState :: s -> (a,s) }
```

State Monad 함수들을 사용하는 방법

```
> :set -package mtl
> import Control.Monad.State
```

get : 결과값과 상태를 모두 입력된 값으로 설정합니다.
```
> runState get 1
(1, 1)
```
put : 결과값을 () 튜플로 바꾸고 상태를 바꿉니다.
```
> runState (put "new state") "initial state"
((), "new state")
```
return : 결과값만 바꾸고 상태는 유지합니다.
```
> runState (return "result") "state"
("result","state")
```
modify : 상태를 바꾸는 함수를 입력값으로 받고 결과로 ((), state)를 반환합니다.
```
> runState (modify (+1)) 1
((),2)
```
gets : 결과값을 바꾸는 함수를 입력값으로 받고 결과로 (value, state)를 반환합니다.
```
> runState (gets (+9)) 2
(11,2)
```
do 표기법을 활용한 state monad 예제. x <- get 에서 x는 결과값입니다.
```
> runState (do { x <- get; return (x + 9) } ) 1
(10,1)
> runState ( do { x <- put 10; return x } ) 1
((),10)
```
runState : 결과값과 상태를 튜플형태로 반환합니다. ( 결과값, 상태 )
```
> runState (return "value") "state"
("value","state")
```
evalState : 결과값을 반환합니다.
```
> evalState (return "value") "state"
"value
```
execState : 상태를 반환합니다.
```
> execState (return "value") "state"
"state"
```

State로 Stack 구현하기
```
import Control.Monad.State

type Stack = [Int]

pop :: State Stack Int
pop = state $ \(x:xs) -> (x, xs)

push :: Int -> State Stack ()
push x = state $ \xs -> ((), x:xs)

stack :: State Stack Int
stack = do
  push 3 -- ((),[3,0,1,2])
  pop -- (3,[0,1,2])
  pop -- (0,[1,2])
```
result)
```
> runState stack [0,1,2]
(0,[1,2])
```