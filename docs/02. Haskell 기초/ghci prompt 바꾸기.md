> [Haskell Snippets](../README.md) / [02. Haskell 기초](README.md) / ghci prompt 바꾸기.md
## ghci prompt 바꾸기
prompt 바꾸기
```
Prelude> :set prompt "λ> "
λ> 
```
multi line prompt 바꾸기
```
λ> :set prompt-cont  "| "
λ> :{
| let x = "x"
| :}
```

unicode로 바꾸기

```
:set prompt  "\x03BB> "
:set prompt-cont "| "
```

prompt를 원래대로 돌려놓기
```
:unset prompt
```

.ghci 파일에 해당 명령들을 지정해 놓으면 ghci가 실행될때마다 prompt를 바꿔주지 않아도 된다.