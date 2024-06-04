[https://gitlab.haskell.org/ghc/ghc/wikis/type-application](https://gitlab.haskell.org/ghc/ghc/wikis/type-application)

[https://kseo.github.io/posts/2017-01-08-visible-type-application-ghc8.html](https://kseo.github.io/posts/2017-01-08-visible-type-application-ghc8.html)

[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeApplications](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-TypeApplications)

"@" 를 사용하여 명시적으로 타입을 표기할수 있게해준다

```
{-# LANGUAGE TypeApplications #-}

answer_read = show (read @Int "3") -- "3" :: String
answer_show = show @Integer (read "5") -- "5" :: String
answer_showread = show @Int (read @Int "7") -- "7" :: String
```