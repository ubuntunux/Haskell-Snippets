> [Haskell Snippets](../README.md) / [01. Haskell 시작하기](README.md) / Cabal로 프로젝트 구성하기.md
## Cabal로 프로젝트 구성하기
[https://wiki.haskell.org/How_to_write_a_Haskell_program#Recommended_tools](https://wiki.haskell.org/How_to_write_a_Haskell_program#Recommended_tools/)

Cabal을 이용하여 하스켈 프로젝트를 구성하는 방법이다. 요즘은 대부분 stack을 추천하는것 같다.

나 역시 cabal은 뭔가 어려워서 좋아하지 않는다.

###Add a build system

Create a .cabal file describing how to build your project:

`$ cabal init`

###Build your project
Now build it! There are two methods of accessing Cabal functionality: through your Setup.hs script or through cabal-install. cabal-install is now the preferred method.

Building using cabal-install and sandboxes, always use sandboxes in your projects unless you are an expert and know what you are doing! Demonstrated here:

    $ cabal sandbox init
    $ cabal install -j

###Generate execute file 

`.cabal-sandbox/bin`