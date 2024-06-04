[http://hackage.haskell.org/package/text-1.2.3.0/docs/Data-Text.html#t:Text](http://hackage.haskell.org/package/text-1.2.3.0/docs/Data-Text.html#t:Text)

[https://ocharles.org.uk/posts/2014-12-17-overloaded-strings.html](https://ocharles.org.uk/posts/2014-12-17-overloaded-strings.html)


```
import Data.Text

a :: String
a = "Hello World"

b :: Text
b = pack "Hello World"
```

In Haskell, the type of a string literal "Hello World" is always String which is defined as [Char] though there are other textual data types such as ByteString and Text. To put it another way, string literals are monomorphic.

GHC provides a language extension called OverloadedStrings. When enabled, literal strings have the type IsString a => a. IsString moudle is defined in Data.String module of base package:

```
class IsString a where
    fromString :: String -> a
```

ByteString and Text are examples of IsString instances, so you can declare the type of string literals as ByteString or Text when OverloadedStrings is enabled.

```
{-# LANGUAGE OverloadedStrings #-}
import Data.Text

a :: String
a = "Hello World"

b :: Text
b = "Hello World"
```


Could not find module ‘Data.Text’ 오류 발생시 .cabal 파일 열어서 아래와 같이 text 모듈을 추가시켜 주자. 

```
build-depends: base >= 4.7 && < 5
             , text
```