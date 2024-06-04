> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Parsing Text - Data.Attoparsec.Text.md
## Parsing Text - Data.Attoparsec.Text
[https://www.schoolofhaskell.com/school/starting-with-haskell/libraries-and-frameworks/text-manipulation/attoparsec](https://www.schoolofhaskell.com/school/starting-with-haskell/libraries-and-frameworks/text-manipulation/attoparsec)

[http://hackage.haskell.org/package/attoparsec-0.13.2.2/docs/Data-Attoparsec-Text.html](http://hackage.haskell.org/package/attoparsec-0.13.2.2/docs/Data-Attoparsec-Text.html)

[http://hackage.haskell.org/package/parsec3-1.0.1.8/docs/Text-Parsec.html](http://hackage.haskell.org/package/parsec3-1.0.1.8/docs/Text-Parsec.html)

Parsec 3 라이브러리와 비교하여 attoparsec은 몇가지 장단점이 있다. 모든 가능성을 염두에두고 만들어 진것이 아니다.
attoparsec은 증분입력(Increamental Input)이 가능하지만 Parsec은 그렇지 않다. 증분입력은 효과적이고 네트웍 보안과 시스템 프로그래밍에서 큰 역활을 한다. Pasec과 비교했을때 attoparsec을 사용함으로써 퍼포먼스 적인 이득을 얻을수 있습니다.

파싱(parse)의 결과로서 IResult를 반환합니다. 실패 할 경우 Fail, 성공할 경우는 Done을 반환합니다. 특정함수의 경우 예를 들면 sepBy, 파싱이 완료된것이 아니라 계속 입력을 제공함으로써 파싱을 이어나갈수 있도록 하는 Partial을 반환하기도 합니다. 이것을 증분입력(Increamental Input)이라고 합니다. Partial의 경우 입력이 실패했거나 입력이 완료된것을 알려줌으로써 최종결과를 얻을수 있습니다. 증분입력이 필요하지 않다면 parseOnly를 사용하면 됩니다.

```
data IResult i r
  Fail i [String] String	
  Partial (i -> IResult i r)	
  Done i r	
```

<br/>
예제) 문자열에서 double형 파싱하기. 파싱이 성공하면 Done "파싱하고남은 문자열" 숫자를 반환한다. 실패하면 Fail을 반환한다.
```
> :set -XOverloadedStrings
> import Data.Attoparsec.Text
> parse double "11 cent"
Done " cent" 11.0
> parse double "ten cent"
Fail "ten cent" [] "Failed reading: takeWhile1"
> parse (string "Hello") "Hello World"
Done " World" "Hello"
```
<br/>
예제) 마치 or연산과 같이 동작하는 함수 <|>를 이용한것이다.
```
> :set -XOverloadedStrings
> import Data.Attoparsec.Text
> import Control.Applicative
> parseOnly (string "hello" <|> string "bye") "hello"
Right "hello"
> parseOnly (string "hello" <|> string "bye") "bye"
Right "bye"
```
<br/>
예제) <\*, \*> 함수의 이용. 어플리커티브 펑터의 특성을 이용한 파싱. 위의 or연산과는 반대로 and연산처럼 동작한다. A <\* B 는 A, B둘다 파싱에 성공할경우 A를 반환하고 A \*> B는 성공할경우 B를 반환한다.

참고:[https://wiki.haskell.org/Typeclassopedia#Applicative](https://wiki.haskell.org/Typeclassopedia#Applicative)
파싱하려는 문자열 앞에 공백이 있기 때문에 "Hello"값을 파싱하는데 실패하여 Left값을 반환한다.
```
> parseOnly (string "Hello") "  Hello, World"
Left "string"
```
아래와 같이 skipSpace 함수를 조합하여 공백을 없애고 파싱하면 Right "Hello"를 반환한다.
```
> parseOnly (skipSpace *> string "Hello") "  Hello, World"
Right "Hello"
```
줄바꿈 문자(\n)로 끝나는 "Hello"에 매칭되면서 결과로 "Hello"를 반환하는 함수를 만들어보자. 여기서 <\* endOfLine 함수 대신에 \*> endOfLine 함수를 사용하면 결과로 그냥 Right ()를 반환하게 된다.
```
> parseOnly (skipSpace *> string "Hello" <* endOfLine) "  Hello\n"
Right "Hello"
```
<br/>
예제) sourceParse함수를 살펴보면 >> 로 연결되어 있는데 모나드 특성을 이용한것으로 string "internet" 부분이 성공하게 되면 return internet을 리턴하도록 되어있는 구조이다.
```
data Source = Internet | Friend | NoAnswer deriving Show

sourceParser :: Parser Source
sourceParser =
      (string "internet" >> return Internet)
  <|> (string "friend" >> return Friend)
  <|> (string "noanswer" >> return NoAnswer)
```
Run)
```
> print $ parseOnly sourceParser "internet"
Right Internet
```
<br/><br/>
예제) 증분입력(Increamental Input) 예제.
```
> r = parse (sepBy double skipSpace) "1 2 3"
> print r
Partial _
> feed r ""
Done "" [1.0,2.0,3.0]
> r2 = feed r "4 5"
> feed r2 ""
Done "" [1.0,2.0,34.0,5.0]
```

<br/>
예제) Monad를 활용한 예제
```
{-# LANGUAGE OverloadedStrings #-}

-- This attoparsec module is intended for parsing text that is
-- represented using an 8-bit character set, e.g. ASCII or ISO-8859-15.
import Data.Attoparsec.Char8
import Data.Word

-- | Type for IP's.
data IP = IP Word8 Word8 Word8 Word8 deriving Show

parseIP :: Parser IP
parseIP = do
  d1 <- decimal
  char '.'
  d2 <- decimal
  char '.'
  d3 <- decimal
  char '.'
  d4 <- decimal
  return $ IP d1 d2 d3 d4
  
> print $ parseOnly parseIP "131.45.68.123"
Right (IP 131 45 68 123)
> Right (IP a b c d) = parseOnly parseIP "131.45.68.123"
> print $ (a, b, c, d)
(131,45,68,123)
```