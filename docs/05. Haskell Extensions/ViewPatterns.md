> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / ViewPatterns.md
## ViewPatterns
[https://ocharles.org.uk/posts/2014-12-02-view-patterns.html](https://ocharles.org.uk/posts/2014-12-02-view-patterns.html)

ViewPattern의 예제. 함수 f는 입력값으로 함수 g를 받고 그결과가 "here"에 매칭이 되면 "correct!"를 반환하는 함수이다. 

```
> :set -XViewPatterns
> g x = "here"
> f (g -> "here") = "correct!"
> f "here"
"correct!"
> f "there"
"*** Exception: <interactive>:29:1-22: Non-exhaustive patterns in function f
```

또다른 예제)

```
lensDownloadsOld :: Map HaskellPackage Int -> Int
lensDownloadsOld packages =
  case M.lookup "lens" packages of
    Just n -> n
    Nothing -> 0
```

이것을 View Pattern으로 바꿔보자. 
아래와 같이 매개변수의 평가결과를 매턴패칭 시킬수 있게된다.

```
{-# LANGUAGE ViewPatterns #-}

lensDownloads :: Map HaskellPackage Int -> Int
lensDownloads (M.lookup "lens" -> Just n) = n
lensDownloads _                           = 0
```

이것을 좀더 일반화 해보자

```
downloadsFor :: HaskellPackage -> Map HaskellPackage Int -> Int
downloadsFor pkg (M.lookup pkg -> Just downloads) = downloads
downloadsFor _   _       
```

또다른 예제)

```
data ViewR a = EmptyR | (Seq a) :> a
viewr :: Seq a -> ViewR a
    
last :: Seq a -> Maybe a
last (viewr -> xs :> x) = Just x
last (viewr -> EmptyR) = Nothing
```
