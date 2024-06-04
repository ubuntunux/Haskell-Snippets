> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Time.md
## Time
[https://two-wrongs.com/haskell-time-library-tutorial.html](https://two-wrongs.com/haskell-time-library-tutorial.html)

[http://hackage.haskell.org/package/time-1.10/docs/Data-Time-Clock.html](http://hackage.haskell.org/package/time-1.10/docs/Data-Time-Clock.html)

```
λ> import Data.Time
λ> formatTime defaultTimeLocale "%F %T" <$> getZonedTime
"2020-06-09 03:05:17"
```

diff file modification time

```
λ> import Data.Time.Clock
λ> import System.Directory
λ> a <- getModificationTime "A.hs"
λ> b <- getModificationTime "B.hs"
λ> diffUTCTime a b
-1716462.870470185s
```