> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / RoleAnnotations.md
## RoleAnnotations
[https://gitlab.haskell.org/ghc/ghc/wikis/roles](https://gitlab.haskell.org/ghc/ghc/wikis/roles)

Role Annotations은 data, newtype, type class에서 선언되는 타입변수의 역활을 지정하는데 사용됩니다. 

타입의 역활에는 phantom / representational 두가지가 있는데 팬텀(phantom)타입은 선언은 되었지만 생성자로 사용되지 않은 타입을 얘기하고 representational 타입은 생성자로 사용된 일반적인 경우를 말한다.
예를들면 Foo a b에서 a는 representational 타입이고 b는 선언만 되었고 생성자로는 사용되지 않았기 때문에 팬텀(phantom)타입이라고 한다.

```
data Foo a b = Foo a
```

RoleAnnotations 확장을 사용하면 명시적으로 phantom / representational 타입을 지정할 수 있다.

```
{-# LANGUAGE RoleAnnotations #-}

type role Foo representational phantom
data Foo a b = Foo a
```

자, 이번엔 나쁜 아이디어를 테스트해보자. phantom타입으로 역활을 지정해놓고 실제로는 생성자로 사용해보자.

```
{-# LANGUAGE RoleAnnotations #-}

type role Foo phantom phantom
data Foo a b = Foo a b

<interactive>:170:24: error:
    • Role mismatch on variable b:
        Annotation says phantom but role representational is required
    • while checking a role annotation for ‘Foo’
```

타입변수 a와 b는 생성자로 사용되었기 때문에 phantom 역활과 맞지 않는다는 에러내용이다.

이번엔 반대의 걍우를 테스트해보자.

```
{-# LANGUAGE RoleAnnotations #-}

type role Foo representational representational
data Foo a b = Foo
```

타입변수 a와 b에 representational 역활을 부여했지만 실제로는 사용되지 않았다. 하지만 이것은 문제없다.
