> [Haskell Snippets](../README.md) / [04. Advanced Haskell](README.md) / hs-boot - break cycle in the module import.md
## hs-boot - break cycle in the module import
GHC supports the compilation of mutually recursive modules. This section explains how.

Every cycle in the module import graph must be broken by a hs-boot file


[https://downloads.haskell.org/ghc/latest/docs/html/users_guide/separate_compilation.html#how-to-compile-mutually-recursive-modules](https://downloads.haskell.org/ghc/latest/docs/html/users_guide/separate_compilation.html#how-to-compile-mutually-recursive-modules)