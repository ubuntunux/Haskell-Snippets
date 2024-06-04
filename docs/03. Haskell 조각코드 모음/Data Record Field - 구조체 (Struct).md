> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Data Record Field - 구조체 (Struct).md
## Data Record Field - 구조체 (Struct)
[https://gist.github.com/mtesseract/1b69087b0aeeb6ddd7023ff05f7b7e68](https://gist.github.com/mtesseract/1b69087b0aeeb6ddd7023ff05f7b7e68)

[http://www.haskellforall.com/2020/07/record-constructors.html?m=1](http://www.haskellforall.com/2020/07/record-constructors.html?m=1)

```
data Person = Person { name :: String
                     , age :: Int
                     } deriving Show

main = do
    print $ Person "Bob" 20
    print $ Person {name = "Alice", age = 30}
    -- print $ Person {name = "Fred"}
    print $ Person {name = "Ann", age = 40}

    let s = Person {name = "Sean", age = 50}
    putStrLn $ name s
    print $ age s

    let s' = s {age = 51}
    print $ age s'
```

run 

```
$ runhaskell structs.hs
Person {name = "Bob", age = 20}
Person {name = "Alice", age = 30}
Person {name = "Ann", age = 40}
Sean
50
51
```

하스켈 언어의 특성상 같은 이름의 record field를 선언할수 없지만 DuplicateRecordFields 확장을 사용하면 가능하다.

```
{-# LANGUAGE DuplicateRecordFields #-}

newtype Foo = Foo { baz :: String }
newtype Bar = Bar { baz :: String }

foo = Foo { baz = "foo text" }
bar = Bar { baz = "bar text" }

main = do
  putStrLn $ "Foo: " ++ baz (foo :: Foo) -- Foo: foo text
  putStrLn $ "Bar: " ++ baz (bar :: Bar) -- Bar: bar text
```

Lens 를 사용할경우 DuplicateRecordFields 만으로는 해결이 되지 않는다. 아래와 같이 여러가지 확장옵션들을 추가해야 하고 또한 makeLenses 함수대신 makeFieldsNoPrefix 함수를 사용해야 한다.

```
{-# LANGUAGE FunctionalDependencies #-}
{-# LANGUAGE MultiParamTypeClasses  #-}
{-# LANGUAGE TemplateHaskell        #-}
{-# LANGUAGE DuplicateRecordFields  #-}
{-# LANGUAGE NoImplicitPrelude      #-}
{-# LANGUAGE TypeSynonymInstances  #-}
{-# LANGUAGE FlexibleInstances  #-}

import Control.Lens

data User  = User  { _name :: Text, _uid :: Int }
data Group = Group { _name :: Text, _gid :: Int }

makeFieldsNoPrefix ''User
makeFieldsNoPrefix ''Group
```