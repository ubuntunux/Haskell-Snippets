[https://stackoverflow.com/questions/32123475/profiling-builds-with-stack](https://stackoverflow.com/questions/32123475/profiling-builds-with-stack)

[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/profiling.html](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/profiling.html)

```
stack clean

stack build --profile

stack exec --profile -- <your program> +RTS <profiling options>

where for <profiling options> you might want -p for time profiling or -h for memory profiling. For time profiling, the profile appears in ./<your program>.prof, and for memory profiling, the profile appears in ./<your program>.hp.

```