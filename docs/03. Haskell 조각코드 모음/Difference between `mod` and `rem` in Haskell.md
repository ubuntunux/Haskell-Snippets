You can really notice the difference when you use a negative number as second parameter and the result is not zero:

```
5 `mod` 3 == 2
5 `rem` 3 == 2

5 `mod` (-3) == -1
5 `rem` (-3) == 2

(-5) `mod` 3 == 1
(-5) `rem` 3 == -2

(-5) `mod` (-3) == -2
(-5) `rem` (-3) == -2
```

Modulus for Fractional
```
> import Data.Fixed
> mod' 2.1. 2.0
0.10000000000000009
```