[https://wiki.haskell.org/Typeclassopedia](https://wiki.haskell.org/Typeclassopedia)

타입클래스의 종류와 인터페이스

    class Eq
    * (==)
    * (/=)
    
    class Ord
    * (>), (>=), (<), (<=)
    * compare
    
    class Show
    * show
    
    class Read
    * read
    
    class Enum
    * pred
    * succ
    ex) ['a'..'e'], [LT..GT]
    
    class Functor
    * fmap
    
    class Applicative
    * pure
    * <*>
    * <$>
    
    class Monoid
    * empty
    * mappend
    * mconcat
    
    class Monad
    * return
    * (>>=)
    * (>>)
    * fail