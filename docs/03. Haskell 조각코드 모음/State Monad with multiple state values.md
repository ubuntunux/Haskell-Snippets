> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / State Monad with multiple state values.md
## State Monad with multiple state values
[https://stackoverflow.com/questions/19227418/state-monad-with-multiple-state-values](https://stackoverflow.com/questions/19227418/state-monad-with-multiple-state-values)

This allows more than one value, but it's hairier :) This is nicely simplified with Daniel's suggestion of Dynamic.
```
import Data.Dynamic
import Data.Maybe
import Control.Monad.State
import Data.Map as M

newtype Ref a = Ref {ref :: Int}

type MutState = State (Int, Map Int Dynamic)

val :: Typeable a => Ref a -> MutState a
val r = snd `fmap` get >>= 
        return . fromJust . (>>= fromDynamic) .  M.lookup (ref r)

new :: Typeable a => a -> MutState (Ref a)
new a = do
  (curr, binds) <- get
  put (curr + 1, M.insert (curr + 1) (toDyn a) binds)
  return . Ref $ curr + 1

set :: Typeable a => Ref a -> a -> MutState ()
set (Ref i) a = do
  (c, m) <- get
  put (c, M.insert i (toDyn a) m)

runMut :: MutState a -> a
runMut = flip evalState (0, M.fromList [])
```

Then to use it
```
default (Int) -- too lazy for signatures :)
test :: Int
test = runMut $ do
  x1 <- new 2
  set x1 3
  x2 <- val x1
  y1 <- new 10
  set y1 20
  y2 <- val y1
  return (x2 + y2)
```

Refs are basically Ints with some type information attached and val will look up the appropriate Dynamic and attempt to force it into the correct type.

If this was real code, you should hide the implementations of Ref and MutState. For convenience, I've fromJusted the return of val bur if you want a safe implementation I suppose you could layer State and Maybe monads to deal with unbound variables.

And in case you are worried about the typeable constraints, as shown above they are trivially derived.

----------------------------------------------------------------------------

There is an implementation already in Control.Monad.State, but it is cumbersome for generality sake: one complication comes from MonadState class, and another from the fact that plain State is implemented in terms of more general StateT.

Here is an example of your task using that implementation. No mutability was used. Note that your example was pasted as is, just adding x prefix:
```
import Control.Monad.State
import qualified Data.Map as M

type MyMap a = M.Map Int a
type MyState a b = State (MyMap a) b
type MyRef = Int

xrun :: MyState a b -> b
xrun x = evalState x (M.empty)

mget :: MyState a (MyMap a)
mget = get

mput :: MyMap a -> MyState a ()
mput = put

mmodify :: (MyMap a -> MyMap a) -> MyState a ()
mmodify x = modify x

xnew :: s -> MyState s MyRef
xnew val = do
    s <- mget
    let newRef = if M.null s then 0 else fst (M.findMax s) + 1
    mput $ M.insert newRef val s
    return newRef

xset :: MyRef -> a -> MyState a () 
xset ref val = modify $ M.insert ref val

xget :: MyRef -> MyState a a
xget ref = fmap (\s -> case M.lookup ref s of Just v -> v) get

test :: MyState Int Int
test = do
  x1 <- xnew 2
  xset x1 3
  x2 <- xget x1
  y1 <- xnew 10
  xset y1 20
  y2 <- xget y1
  return (x2 + y2)

main = print $ xrun test
```