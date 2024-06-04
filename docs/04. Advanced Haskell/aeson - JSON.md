[https://github.com/bos/aeson](https://github.com/bos/aeson)

[https://www.schoolofhaskell.com/school/starting-with-haskell/libraries-and-frameworks/text-manipulation/json](https://www.schoolofhaskell.com/school/starting-with-haskell/libraries-and-frameworks/text-manipulation/json)

[https://hackage.haskell.org/package/aeson-1.4.7.1/docs/Data-Aeson.html](https://hackage.haskell.org/package/aeson-1.4.7.1/docs/Data-Aeson.html)

[https://hackage.haskell.org/package/aeson-1.4.7.1/docs/Data-Aeson-Types.html#t:Value](https://hackage.haskell.org/package/aeson-1.4.7.1/docs/Data-Aeson-Types.html#t:Value)

[https://hackage.haskell.org/package/unordered-containers-0.2.10.0/docs/Data-HashMap-Strict.html#t:HashMap](https://hackage.haskell.org/package/unordered-containers-0.2.10.0/docs/Data-HashMap-Strict.html#t:HashMap)

```
{-# LANGUAGE OverloadedStrings #-}

import Data.Aeson
import Data.Aeson.Types
import qualified Data.HashMap.Strict as HM
import Data.Scientific

Just (Object obj) = decode "{\"foo\": 123}" :: Maybe Value
xs = HM.elems obj
x = xs !! 0
Number f = x
toRealFloat f::Float
```