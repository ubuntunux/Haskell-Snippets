[http://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#t:Enum](http://hackage.haskell.org/package/base-4.12.0.0/docs/Prelude.html#t:Enum)

```
λ> data MyDataType = Foo | Bar | Baz deriving (Enum, Bounded, Show, Eq, Ord)
λ> Foo < Bar
True
λ> Foo == Bar
False
λ> Foo > Bar
False
λ> [Foo ..]
[Foo,Bar,Baz]
λ> succ Foo
Bar
λ> pred Bar
Foo
λ> map fromEnum [Foo ..]
[0,1,2]
λ> map toEnum [0,1,2]::[MyDataType]
[Foo,Bar,Baz]
λ> minBound::MyDataType
Foo
λ> maxBound::MyDataType
Baz
λ> fromEnum (maxBound::MyDataType)
2
```

```
succ                   = toEnum . (+ 1)  . fromEnum
pred                   = toEnum . (subtract 1) . fromEnum
enumFrom x             = map toEnum [fromEnum x ..]
enumFromThen x y       = map toEnum [fromEnum x, fromEnum y ..]
enumFromTo x y         = map toEnum [fromEnum x .. fromEnum y]
enumFromThenTo x1 x2 y = map toEnum [fromEnum x1, fromEnum x2 .. fromEnum y]
```