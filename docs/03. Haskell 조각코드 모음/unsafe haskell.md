[http://hackage.haskell.org/package/base-4.12.0.0/docs/GHC-Exts.html#v:unsafeCoerce-35-](http://hackage.haskell.org/package/base-4.12.0.0/docs/GHC-Exts.html#v:unsafeCoerce-35-)

#### unsafeCoerce
The highly unsafe primitive unsafeCoerce converts a value from any type to any other type. Needless to say, if you use this function, it is your responsibility to ensure that the old and new types have identical internal representations, in order to prevent runtime corruption.
```
λ> :set -XMagicHash
λ> import GHC.Exts
λ> unsafeCoerce# (True :: Bool) :: Int
-8646910738190095638
```