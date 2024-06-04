> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Formatting.md
## Formatting
[http://hackage.haskell.org/package/base-4.12.0.0/docs/Text-Printf.html](http://hackage.haskell.org/package/base-4.12.0.0/docs/Text-Printf.html)

```
>>> import Text.Printf
>>> printf "%s, %d, %.4f" "hello" 123 pi
hello, 123, 3.1416
```

```
c      character               Integral
d      decimal                 Integral
o      octal                   Integral
x      hexadecimal             Integral
X      hexadecimal             Integral
b      binary                  Integral
u      unsigned decimal        Integral
f      floating point          RealFloat
F      floating point          RealFloat
g      general format float    RealFloat
G      general format float    RealFloat
e      exponent format float   RealFloat
E      exponent format float   RealFloat
s      string                  String
v      default format          any type
```