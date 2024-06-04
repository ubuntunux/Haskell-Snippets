[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/using-warnings.html?highlight=unused-top-binds](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/using-warnings.html?highlight=unused-top-binds)

GHC has a number of options that select which types of non-fatal error messages, otherwise known as warnings, can be generated during compilation. Some options control individual warnings and others control collections of warnings. To turn off an individual warning -W<wflag>, use -Wno-<wflag>. To reverse``-Werror``, which makes all warnings into errors, use -Wwarn.

By default, you get a standard set of warnings which are generally likely to indicate bugs in your program. These are:

```
-Woverlapping-patterns
-Wwarnings-deprecations
-Wdeprecations
-Wdeprecated-flags
-Wunrecognised-pragmas
-Wduplicate-exports
-Woverflowed-literals
-Wempty-enumerations
-Wmissing-fields
-Wmissing-methods
-Wwrong-do-bind
-Wsimplifiable-class-constraints
-Wtyped-holes
-Wdeferred-type-errors
-Wpartial-type-signatures
-Wunsupported-calling-conventions
-Wdodgy-foreign-imports
-Winline-rule-shadowing
-Wunsupported-llvm-version
-Wmissed-extra-shared-lib
-Wtabs
-Wunrecognised-warning-flags
-Winaccessible-code
-Wstar-is-type
-Wstar-binder
-Wspace-after-bang
The following flags are simple ways to select standard “packages” of warnings:
```
