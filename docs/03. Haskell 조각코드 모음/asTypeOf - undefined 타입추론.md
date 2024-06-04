> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / asTypeOf - undefined 타입추론.md
## asTypeOf - undefined 타입추론
```
import qualified Data.Vector.Storable as SVector
import Foreign.Storable

sizeOfElem :: (Storable a) => SVector.Vector a -> Int
sizeOfElem vec = sizeOf (undefined `asTypeOf` SVector.head vec)
```

Result

```
λ> sizeOfElem $ SVector.fromList ([]::[Int])
8
```