    import System.Directory
    
    data Permissions
      = System.Directory.Permissions {readable :: Bool,
                                      writable :: Bool,
                                      executable :: Bool,
                                      searchable :: Bool}
    canonicalizePath :: FilePath -> IO FilePath
    copyFile :: FilePath -> FilePath -> IO ()
    copyPermissions :: FilePath -> FilePath -> IO ()
    createDirectory :: FilePath -> IO ()
    createDirectoryIfMissing :: Bool -> FilePath -> IO ()
    doesDirectoryExist :: FilePath -> IO Bool
    doesFileExist :: FilePath -> IO Bool
    emptyPermissions :: Permissions
    findExecutable :: String -> IO (Maybe FilePath)
    findExecutables :: String -> IO [FilePath]
    findFile :: [FilePath] -> String -> IO (Maybe FilePath)
    findFiles :: [FilePath] -> String -> IO [FilePath]
    findFilesWith ::
      (FilePath -> IO Bool) -> [FilePath] -> String -> IO [FilePath]
    getAppUserDataDirectory :: String -> IO FilePath
    getCurrentDirectory :: IO FilePath
    getDirectoryContents :: FilePath -> IO [FilePath]
    getHomeDirectory :: IO FilePath
    getModificationTime ::
      FilePath -> IO time-1.5.0.1:Data.Time.Clock.UTC.UTCTime
    getPermissions :: FilePath -> IO Permissions
    getTemporaryDirectory :: IO FilePath
    getUserDocumentsDirectory :: IO FilePath
    makeAbsolute :: FilePath -> IO FilePath
    makeRelativeToCurrentDirectory :: FilePath -> IO FilePath
    removeDirectory :: FilePath -> IO ()
    removeDirectoryRecursive :: FilePath -> IO ()
    removeFile :: FilePath -> IO ()
    renameDirectory :: FilePath -> FilePath -> IO ()
    renameFile :: FilePath -> FilePath -> IO ()
    setCurrentDirectory :: FilePath -> IO ()
    setOwnerExecutable :: Bool -> Permissions -> Permissions
    setOwnerReadable :: Bool -> Permissions -> Permissions
    setOwnerSearchable :: Bool -> Permissions -> Permissions
    setOwnerWritable :: Bool -> Permissions -> Permissions
    setPermissions :: FilePath -> Permissions -> IO ()
    
        
