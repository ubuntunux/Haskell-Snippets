```{.haskell}
import System.FilePath

(-<.>) :: FilePath -> String -> FilePath
(<.>) :: FilePath -> String -> FilePath
(</>) :: FilePath -> FilePath -> FilePath
type FilePath = String
addExtension :: FilePath -> String -> FilePath
addTrailingPathSeparator :: FilePath -> FilePath
combine :: FilePath -> FilePath -> FilePath
dropDrive :: FilePath -> FilePath
dropExtension :: FilePath -> FilePath
dropExtensions :: FilePath -> FilePath
dropFileName :: FilePath -> FilePath
dropTrailingPathSeparator :: FilePath -> FilePath
equalFilePath :: FilePath -> FilePath -> Bool
extSeparator :: Char
getSearchPath :: IO [FilePath]
hasDrive :: FilePath -> Bool
hasExtension :: FilePath -> Bool
hasTrailingPathSeparator :: FilePath -> Bool
isAbsolute :: FilePath -> Bool
isDrive :: FilePath -> Bool
isExtSeparator :: Char -> Bool
isPathSeparator :: Char -> Bool
isRelative :: FilePath -> Bool
isSearchPathSeparator :: Char -> Bool
isValid :: FilePath -> Bool
joinDrive :: FilePath -> FilePath -> FilePath
joinPath :: [FilePath] -> FilePath
makeRelative :: FilePath -> FilePath -> FilePath
makeValid :: FilePath -> FilePath
normalise :: FilePath -> FilePath
pathSeparator :: Char
pathSeparators :: [Char]
replaceBaseName :: FilePath -> String -> FilePath
replaceDirectory :: FilePath -> String -> FilePath
replaceExtension :: FilePath -> String -> FilePath
replaceFileName :: FilePath -> String -> FilePath
searchPathSeparator :: Char
splitDirectories :: FilePath -> [FilePath]
splitDrive :: FilePath -> (FilePath, FilePath)
splitExtension :: FilePath -> (String, String)
splitExtensions :: FilePath -> (FilePath, String)
splitFileName :: FilePath -> (String, String)
splitPath :: FilePath -> [FilePath]
splitSearchPath :: String -> [FilePath]
takeBaseName :: FilePath -> String
takeDirectory :: FilePath -> FilePath
takeDrive :: FilePath -> FilePath
takeExtension :: FilePath -> String
takeExtensions :: FilePath -> String
takeFileName :: FilePath -> FilePath
```