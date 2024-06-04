[https://stackoverflow.com/questions/31251163/what-is-the-flexiblecontexts-extension-good-for-could-you-please-explain-it-usi?answertab=votes#tab-top](https://stackoverflow.com/questions/31251163/what-is-the-flexiblecontexts-extension-good-for-could-you-please-explain-it-usi?answertab=votes#tab-top)

FlexibleContexts, FlexibleInstances 이 두가지 확장은 비슷한 성격을 가지는데, 바로 typeclass의 instance나 함수를 선언시 Num a => a 등과 같이 Num 타입클래스에 대해 타입변수 a를 사용해야만 하는데 FlexibleContexts, FlexibleInstances 확장을 사용하면 Num Int, Num Float 처럼 좀더 명확하고 원하는 타입에 대해서만 동작이 가능하도록 해준다.

함수정의에서 FlexibleContexts가 없다면 타입클래스는 반드시 타입변수를 선언해야만 한다. 

```
add :: Num a => a -> a -> a 
add = (+)
```

예를들면 Num 타입클래스에 대해 타입변수 a를 선언해야만 한다. 하지만 FlexibleContexts가 활성화 되면 타입클래스에 대해 타입을 사용할수 있게 된다. ( Int, String, Char등 )

```
intAdd :: Num Int => Int -> Int -> Int 
intAdd = (+)
```

이 예제는 최대한 단순하게 고안된 것이다. FlexibleContexts는 보통 MultiParamTypeClasses 확장과 함께 사용될때 유용합니다. 예제입니다.

```
class Shower a b where
  myShow :: a -> b

doSomething :: Shower a String => a -> String
doSomething = myShow
```

여기 예제를 보면 오직 Shower a String 타입에 대해서만 myShow 함수가 동작하도록 되어 있습니다. FlexibleContexts 확장이 없다면 String concrete type 타입 대신 타입변수를 사용해야한 할것입니다.