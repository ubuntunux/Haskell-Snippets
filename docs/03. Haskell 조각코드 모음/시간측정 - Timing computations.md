[https://wiki.haskell.org/Timing_computations](https://wiki.haskell.org/Timing_computations)

```
import Text.Printf
import Control.Exception
import System.CPUTime

time :: IO t -> IO t
time a = do
    start <- getCPUTime
    v <- a
    end   <- getCPUTime
    let diff = (fromIntegral (end - start)) / (10^12)
    printf "Computation time: %0.3f sec\n" (diff :: Double)
    return v

main = do
    putStrLn "Starting..."
    time $ product [1..10000] `seq` return ()
    putStrLn "Done."
```

```
import Text.Printf
import Control.Exception
import System.CPUTime
import Control.Parallel.Strategies
import Control.Monad
import System.Environment

lim :: Int
lim = 10^6

time :: (Num t, NFData t) => t -> IO ()
time y = do
    start <- getCPUTime
    replicateM_ lim $ do
        x <- evaluate $ 1 + y
        rnf x `seq` return ()
    end   <- getCPUTime
    let diff = (fromIntegral (end - start)) / (10^12)
    printf "Computation time: %0.9f sec\n" (diff :: Double)
    printf "Individual time: %0.9f sec\n" (diff / fromIntegral lim :: Double)
    return ()

main = do
    [n] <- getArgs
    let y = read n
    putStrLn "Starting..."
    time (y :: Int)
    putStrLn "Done."
```