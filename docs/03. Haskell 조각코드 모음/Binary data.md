> [Haskell Snippets](../README.md) / [03. Haskell 조각코드 모음](README.md) / Binary data.md
## Binary data
[https://wiki.haskell.org/Dealing_with_binary_data](https://wiki.haskell.org/Dealing_with_binary_data)

[https://hackage.haskell.org/package/binary-0.8.7.0/docs/Data-Binary.html](https://hackage.haskell.org/package/binary-0.8.7.0/docs/Data-Binary.html)

[https://stackoverflow.com/questions/32253948/haskell-read-write-binary-files-complete-working-example](https://stackoverflow.com/questions/32253948/haskell-read-write-binary-files-complete-working-example)

[https://livebook.manning.com/book/get-programming-with-haskell/chapter-25/39](https://livebook.manning.com/book/get-programming-with-haskell/chapter-25/39)


```
Storable Vector -> Binary

SVector.toList (SVector.unsafeCast vertices::SVector.Vector Char)
```