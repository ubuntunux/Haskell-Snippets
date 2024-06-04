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