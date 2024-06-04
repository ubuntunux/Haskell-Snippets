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