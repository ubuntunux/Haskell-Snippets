> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / BinaryLiterals.md
## BinaryLiterals
```
{-# LANGUAGE BinaryLiterals #-}

-- | should print:
--   @(1458,1458,1458,1458)@
main = print (1458, 0x5B2, 0o2662, 0b10110110010)
```