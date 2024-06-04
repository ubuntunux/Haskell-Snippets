#### Unicode를 ByteCode로 다루기

    import Prelude hiding (putStr) -- putStr을 숨긴다.
    import Data.ByteString.Char8 (putStr) -- ByteCode에 맞는 putStr을 불러온다.
    import Data.ByteString.UTF8 (fromString)
    
    main :: IO ()
    main = putStr $ fromString "이것은 유니코드"



#### utf8 파일 불러오기

    import System.IO
    
    f <- openFile name ReadMode
    hSetEncoding f utf8
    hGetContents f
    hClose f