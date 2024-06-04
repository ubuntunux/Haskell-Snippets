[https://connectionrequired.com/blog/2009/07/my-first-introduction-to-haskell-extensions-flexibleinstances](https://connectionrequired.com/blog/2009/07/my-first-introduction-to-haskell-extensions-flexibleinstances)

typeclass의 instance를 정의할때 타입변수를 두번 나타낼수 있게 해줍니다.

```
class Something a where
  doSomething :: a -> Integer

instance Something Integer where
  doSomething x = 1

instance Something Char where
  doSomething x = 2
```

여기까지는 문제가 없다. 

```
instance Something [Char] where
  doSomething x = 3
```

하지만 [Char] 타입에 대한 instance를 정의하면 에러를 보게될것이다.

```
Illegal instance declaration for `Something [Char]'
  (All instance types must be of the form (T a1 ... an)
  where a1 ... an are type *variables*,
  and each type variable appears at most once in the instance head.
  Use -XFlexibleInstances if you want to disable this.)
In the instance declaration for `Something [Char]'
```

대략  instance 선언부에 타입변수는 한번만 나타낼수 있고 -XFlexibleInstances를 사용하면 이것을 불가능하게 할수 있다라는 메시지인듯 하다. [Char] 를 Something의 instance로 만들수는 없지만 [a]를 가지고 만들수 있다.

```
instance Something [a] where
  doSomething x = 3
  -- ghci> doSomething "test"
  -- 3
```

[Char] 타입에 대해서 문제없이 잘 동작한다.하지만 의도치 않게 [Int], [Float]등 모든 타입 a에 대해서도 동작하게 된다. 그러면 [Char], [Int], [Float]등이 모두 같은 동작을 하게 되버린다. 
즉, 이런경우에 FlexibleInstances 확장을 사용하면 되고 [Char], [Int], [Float]타입에 대한 instance를 선언할 수 있게 해준다.