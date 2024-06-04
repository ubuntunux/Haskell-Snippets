> [Haskell Snippets](../README.md) / [02. Haskell 기초](README.md) / Pragma의 종류 ( GHC 확장 및 옵션 설정방법 ).md
## Pragma의 종류 ( GHC 확장 및 옵션 설정방법 )
[https://downloads.haskell.org/~ghc/7.0.3/docs/html/users_guide/pragmas.html](https://downloads.haskell.org/~ghc/7.0.3/docs/html/users_guide/pragmas.html)

소스코드에서 설정하는 방법

```
{-# LANGUAGE DataKinds         #-}
{-# LANGUAGE TypeApplications  #-}
{-# OPTIONS_GHC -fno-warn-redundant-constraints #-}
{-# OPTIONS_GHC -fobject-code #-}
```

ghci에서 사용하는 방법
```
:set -XDataKinds 
:set -XTypeApplications
:set -fobject-code
:set -fno-warn-redundant-constraints
```

.ghci 파일에 설정 방법
```
:set -fobject-code
:set -XDataKinds
:set -XTypeApplications
:set -fobject-code
:set -fno-warn-redundant-constraints
```