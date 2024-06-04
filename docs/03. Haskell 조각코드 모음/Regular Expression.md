> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Regular Expression.md
## Regular Expression
[http://hackage.haskell.org/package/regex-posix-0.96.0.0/docs/Text-Regex-Posix.html](http://hackage.haskell.org/package/regex-posix-0.96.0.0/docs/Text-Regex-Posix.html)

[http://book.realworldhaskell.org/read/efficient-file-processing-regular-expressions-and-file-name-matching.html](http://book.realworldhaskell.org/read/efficient-file-processing-regular-expressions-and-file-name-matching.html)

[http://www.serpentine.com/blog/2007/02/27/a-haskell-regular-expression-tutorial/](http://www.serpentine.com/blog/2007/02/27/a-haskell-regular-expression-tutorial/)


```
λ>  "foo 123 baz" =~ "([a-z]+) *([0-9]+) *([a-z]+)" :: (String,String,String,[String])
("","foo 123 baz","",["foo","123","baz"])
λ> let (_,_,_,[group1,group2,group3]) = "foo 123 baz" =~ "([a-z]+) *([0-9]+) *([a-z]+)" :: (String,String,String,[String])
λ> group1
"foo"
λ> group2
"123"
λ> group3
"baz"
```