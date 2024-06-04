아주 간단한 연산을 한가지 해보자. 1과 -1을 서로 더하면 0이 되겠지만 Haskell에서는 -가 함수로 간주되기 때문에 1 + -1을 실행하면 에러가 발생한다.
```
> 1 + -1
<interactive>:1:1: error:
    Precedence parsing error
        cannot mix ‘+’ [infixl 6] and prefix `-' [infixl 6] in the same infix expression
```
아래와 같이 음수(-)를 괄호로 감싸면 괜찮다.
```
> 1 + (-1)
0
```
또는 NegativeLiterals 확장을 사용하면 -를 음수기호로 사용할수 있게 된다.
```
> :set -XNegativeLiterals
> 1 + -1
0
```