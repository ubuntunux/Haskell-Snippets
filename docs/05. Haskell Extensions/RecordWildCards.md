> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / RecordWildCards.md
## RecordWildCards
[https://ocharles.org.uk/posts/2014-12-04-record-wildcards.html](https://ocharles.org.uk/posts/2014-12-04-record-wildcards.html)
[https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RecordWildCards](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#extension-RecordWildCards)

아래와 같이 data의 멤버변수를 쉽게 접근할 수 있다.

```
{-# LANGUAGE RecordWildCards #-}

data Client = Client { firstName     :: String
                     , lastName      :: String
                     , clientID      :: String 
                     } deriving (Show)

printClientName :: Client -> IO ()
printClientName client@Client{..} = do
    putStrLn firstName
    putStrLn lastName
    putStrLn clientID
```