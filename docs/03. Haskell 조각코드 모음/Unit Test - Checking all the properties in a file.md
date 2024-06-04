> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Unit Test - Checking all the properties in a file.md
## Unit Test - Checking all the properties in a file
quickCheckAll is a Template Haskell helper which finds all the definitions in the current file whose name begins with prop_ and tests them.

```
{-# LANGUAGE TemplateHaskell #-}

import Test.QuickCheck (quickCheckAll)
import Data.List (sort)

idempotent :: Eq a => (a -> a) -> a -> Bool
idempotent f x = f (f x) == f x

prop_sortIdempotent = idempotent sort

-- does not begin with prop_, will not be picked up by the test runner
sortDoesNotChangeLength xs = length (sort xs) == length xs


return []
main = $quickCheckAll
```

Note that the return [] line is required. It makes definitions textually above that line visible to Template Haskell.

```
$ runhaskell QuickCheckAllExample.hs
=== prop_sortIdempotent from tree.hs:7 ===
+++ OK, passed 100 tests.
```


