> [Haskell Snippets](../README.md) / [02. Haskell 기초](README.md) / Weak Head Normal Foam - WHNF.md
## Weak Head Normal Foam - WHNF
[https://opentutorials.org/module/1941/11208](https://opentutorials.org/module/1941/11208)

Haskell에서는 식을 평가할 때 식이 Weak Head Normal Form(WHNF)의 형태가 될 때까지만 평가합니다. WHNF는 다음과 같은 형태를 가진 표현식을 의미합니다.

- 식의 맨 바깥쪽이 값 생성자인 식.
- 식의 맨 바깥쪽이 람다 표현식인 식.

따라서 다음 식들은 WHNF입니다.
```
(\x -> x + 2) -- 식의 맨 바깥이 람다 표현식이므로 WHNF.
Just (2+3) -- 식의 맨 바깥쪽이 값 생성자(Just)이므로 WHNF.
'L' : ("am" ++ "bda") -- 식의 맨 바깥쪽이 값 생성자(:)이므로 WHNF.
```

반면에 다음 식들은 WHNF가 아닙니다.
```
(\x -> x + 2) 3 -- 함수에 어떤 인자값을 적용하는 식이므로 WHNF가 아닙니다.
1 + 2 -- 역시 함수(+)에 어떤 인자값을 적용하는 식이므로 WHNF가 아닙니다.
"Lam" ++ "bda" -- 역시 함수(++)에 어떤 인자값을 적용하는 식이므로 WHNF가 아닙니다.
```