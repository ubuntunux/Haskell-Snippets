[https://hackage.haskell.org/package/bytestring-0.10.8.2/docs/Data-ByteString.html](https://hackage.haskell.org/package/bytestring-0.10.8.2/docs/Data-ByteString.html)

[https://hackage.haskell.org/package/bytestring-0.10.10.1/docs/Data-ByteString-Char8.html](https://hackage.haskell.org/package/bytestring-0.10.10.1/docs/Data-ByteString-Char8.html)


```
empty :: ByteStringSource#

O(1) The empty ByteString

singleton :: Word8 -> ByteStringSource#

O(1) Convert a Word8 into a ByteString

pack :: [Word8] -> ByteStringSource#

O(n) Convert a [Word8] into a ByteString.

For applications with large numbers of string literals, pack can be a bottleneck. In such cases, consider using packAddress (GHC only).

unpack :: ByteString -> [Word8]Source#

O(n) Converts a ByteString to a [Word8].

Basic interface
cons :: Word8 -> ByteString -> ByteString infixr 5Source#

O(n) cons is analogous to (:) for lists, but of different complexity, as it requires making a copy.

snoc :: ByteString -> Word8 -> ByteString infixl 5Source#

O(n) Append a byte to the end of a ByteString

append :: ByteString -> ByteString -> ByteStringSource#

O(n) Append two ByteStrings

head :: ByteString -> Word8Source#

O(1) Extract the first element of a ByteString, which must be non-empty. An exception will be thrown in the case of an empty ByteString.

uncons :: ByteString -> Maybe (Word8, ByteString)Source#

O(1) Extract the head and tail of a ByteString, returning Nothing if it is empty.

unsnoc :: ByteString -> Maybe (ByteString, Word8)Source#

O(1) Extract the init and last of a ByteString, returning Nothing if it is empty.

last :: ByteString -> Word8Source#

O(1) Extract the last element of a ByteString, which must be finite and non-empty. An exception will be thrown in the case of an empty ByteString.

tail :: ByteString -> ByteStringSource#

O(1) Extract the elements after the head of a ByteString, which must be non-empty. An exception will be thrown in the case of an empty ByteString.

init :: ByteString -> ByteStringSource#

O(1) Return all the elements of a ByteString except the last one. An exception will be thrown in the case of an empty ByteString.

null :: ByteString -> BoolSource#

O(1) Test whether a ByteString is empty.

length :: ByteString -> IntSource#

O(1) length returns the length of a ByteString as an Int.

Transforming ByteStrings
map :: (Word8 -> Word8) -> ByteString -> ByteStringSource#

O(n) map f xs is the ByteString obtained by applying f to each element of xs.

reverse :: ByteString -> ByteStringSource#

O(n) reverse xs efficiently returns the elements of xs in reverse order.

intersperse :: Word8 -> ByteString -> ByteStringSource#

O(n) The intersperse function takes a Word8 and a ByteString and `intersperses' that byte between the elements of the ByteString. It is analogous to the intersperse function on Lists.

intercalate :: ByteString -> [ByteString] -> ByteStringSource#

O(n) The intercalate function takes a ByteString and a list of ByteStrings and concatenates the list after interspersing the first argument between each element of the list.

transpose :: [ByteString] -> [ByteString]Source#

The transpose function transposes the rows and columns of its ByteString argument.

Reducing ByteStrings (folds)
foldl :: (a -> Word8 -> a) -> a -> ByteString -> aSource#

foldl, applied to a binary operator, a starting value (typically the left-identity of the operator), and a ByteString, reduces the ByteString using the binary operator, from left to right.

foldl' :: (a -> Word8 -> a) -> a -> ByteString -> aSource#

foldl' is like foldl, but strict in the accumulator.

foldl1 :: (Word8 -> Word8 -> Word8) -> ByteString -> Word8Source#

foldl1 is a variant of foldl that has no starting value argument, and thus must be applied to non-empty ByteStrings. An exception will be thrown in the case of an empty ByteString.

foldl1' :: (Word8 -> Word8 -> Word8) -> ByteString -> Word8Source#

'foldl1\'' is like foldl1, but strict in the accumulator. An exception will be thrown in the case of an empty ByteString.

foldr :: (Word8 -> a -> a) -> a -> ByteString -> aSource#

foldr, applied to a binary operator, a starting value (typically the right-identity of the operator), and a ByteString, reduces the ByteString using the binary operator, from right to left.

foldr' :: (Word8 -> a -> a) -> a -> ByteString -> aSource#

foldr' is like foldr, but strict in the accumulator.

foldr1 :: (Word8 -> Word8 -> Word8) -> ByteString -> Word8Source#

foldr1 is a variant of foldr that has no starting value argument, and thus must be applied to non-empty ByteStrings An exception will be thrown in the case of an empty ByteString.

foldr1' :: (Word8 -> Word8 -> Word8) -> ByteString -> Word8Source#

'foldr1\'' is a variant of foldr1, but is strict in the accumulator.

Special folds
concat :: [ByteString] -> ByteStringSource#

O(n) Concatenate a list of ByteStrings.

concatMap :: (Word8 -> ByteString) -> ByteString -> ByteStringSource#

Map a function over a ByteString and concatenate the results

any :: (Word8 -> Bool) -> ByteString -> BoolSource#

O(n) Applied to a predicate and a ByteString, any determines if any element of the ByteString satisfies the predicate.

all :: (Word8 -> Bool) -> ByteString -> BoolSource#

O(n) Applied to a predicate and a ByteString, all determines if all elements of the ByteString satisfy the predicate.

maximum :: ByteString -> Word8Source#

O(n) maximum returns the maximum value from a ByteString This function will fuse. An exception will be thrown in the case of an empty ByteString.

minimum :: ByteString -> Word8Source#

O(n) minimum returns the minimum value from a ByteString This function will fuse. An exception will be thrown in the case of an empty ByteString.

Building ByteStrings
Scans
scanl :: (Word8 -> Word8 -> Word8) -> Word8 -> ByteString -> ByteStringSource#

scanl is similar to foldl, but returns a list of successive reduced values from the left. This function will fuse.

scanl f z [x1, x2, ...] == [z, z `f` x1, (z `f` x1) `f` x2, ...]
Note that

last (scanl f z xs) == foldl f z xs.
scanl1 :: (Word8 -> Word8 -> Word8) -> ByteString -> ByteStringSource#

scanl1 is a variant of scanl that has no starting value argument. This function will fuse.

scanl1 f [x1, x2, ...] == [x1, x1 `f` x2, ...]
scanr :: (Word8 -> Word8 -> Word8) -> Word8 -> ByteString -> ByteStringSource#

scanr is the right-to-left dual of scanl.

scanr1 :: (Word8 -> Word8 -> Word8) -> ByteString -> ByteStringSource#

scanr1 is a variant of scanr that has no starting value argument.

Accumulating maps
mapAccumL :: (acc -> Word8 -> (acc, Word8)) -> acc -> ByteString -> (acc, ByteString)Source#

The mapAccumL function behaves like a combination of map and foldl; it applies a function to each element of a ByteString, passing an accumulating parameter from left to right, and returning a final value of this accumulator together with the new list.

mapAccumR :: (acc -> Word8 -> (acc, Word8)) -> acc -> ByteString -> (acc, ByteString)Source#

The mapAccumR function behaves like a combination of map and foldr; it applies a function to each element of a ByteString, passing an accumulating parameter from right to left, and returning a final value of this accumulator together with the new ByteString.

Generating and unfolding ByteStrings
replicate :: Int -> Word8 -> ByteStringSource#

O(n) replicate n x is a ByteString of length n with x the value of every element. The following holds:

replicate w c = unfoldr w (\u -> Just (u,u)) c
This implemenation uses memset(3)

unfoldr :: (a -> Maybe (Word8, a)) -> a -> ByteStringSource#

O(n), where n is the length of the result. The unfoldr function is analogous to the List 'unfoldr'. unfoldr builds a ByteString from a seed value. The function takes the element and returns Nothing if it is done producing the ByteString or returns Just (a,b), in which case, a is the next byte in the string, and b is the seed value for further production.

Examples:

   unfoldr (\x -> if x <= 5 then Just (x, x + 1) else Nothing) 0
== pack [0, 1, 2, 3, 4, 5]
unfoldrN :: Int -> (a -> Maybe (Word8, a)) -> a -> (ByteString, Maybe a)Source#

O(n) Like unfoldr, unfoldrN builds a ByteString from a seed value. However, the length of the result is limited by the first argument to unfoldrN. This function is more efficient than unfoldr when the maximum length of the result is known.

The following equation relates unfoldrN and unfoldr:

fst (unfoldrN n f s) == take n (unfoldr f s)
Substrings
Breaking strings
take :: Int -> ByteString -> ByteStringSource#

O(1) take n, applied to a ByteString xs, returns the prefix of xs of length n, or xs itself if n > length xs.

drop :: Int -> ByteString -> ByteStringSource#

O(1) drop n xs returns the suffix of xs after the first n elements, or [] if n > length xs.

splitAt :: Int -> ByteString -> (ByteString, ByteString)Source#

O(1) splitAt n xs is equivalent to (take n xs, drop n xs).

takeWhile :: (Word8 -> Bool) -> ByteString -> ByteStringSource#

takeWhile, applied to a predicate p and a ByteString xs, returns the longest prefix (possibly empty) of xs of elements that satisfy p.

dropWhile :: (Word8 -> Bool) -> ByteString -> ByteStringSource#

dropWhile p xs returns the suffix remaining after takeWhile p xs.

span :: (Word8 -> Bool) -> ByteString -> (ByteString, ByteString)Source#

span p xs breaks the ByteString into two segments. It is equivalent to (takeWhile p xs, dropWhile p xs)

spanEnd :: (Word8 -> Bool) -> ByteString -> (ByteString, ByteString)Source#

spanEnd behaves like span but from the end of the ByteString. We have

spanEnd (not.isSpace) "x y z" == ("x y ","z")
and

spanEnd (not . isSpace) ps
   ==
let (x,y) = span (not.isSpace) (reverse ps) in (reverse y, reverse x)
break :: (Word8 -> Bool) -> ByteString -> (ByteString, ByteString)Source#

break p is equivalent to span (not . p).

Under GHC, a rewrite rule will transform break (==) into a call to the specialised breakByte:

break ((==) x) = breakByte x
break (==x) = breakByte x
breakEnd :: (Word8 -> Bool) -> ByteString -> (ByteString, ByteString)Source#

breakEnd behaves like break but from the end of the ByteString

breakEnd p == spanEnd (not.p)

group :: ByteString -> [ByteString]Source#

The group function takes a ByteString and returns a list of ByteStrings such that the concatenation of the result is equal to the argument. Moreover, each sublist in the result contains only equal elements. For example,

group "Mississippi" = ["M","i","ss","i","ss","i","pp","i"]
It is a special case of groupBy, which allows the programmer to supply their own equality test. It is about 40% faster than groupBy (==)

groupBy :: (Word8 -> Word8 -> Bool) -> ByteString -> [ByteString]Source#

The groupBy function is the non-overloaded version of group.

inits :: ByteString -> [ByteString]Source#

O(n) Return all initial segments of the given ByteString, shortest first.

tails :: ByteString -> [ByteString]Source#

O(n) Return all final segments of the given ByteString, longest first.

stripPrefix :: ByteString -> ByteString -> Maybe ByteStringSource#

O(n) The stripPrefix function takes two ByteStrings and returns Just the remainder of the second iff the first is its prefix, and otherwise Nothing.

Since: 0.10.8.0

stripSuffix :: ByteString -> ByteString -> Maybe ByteStringSource#

O(n) The stripSuffix function takes two ByteStrings and returns Just the remainder of the second iff the first is its suffix, and otherwise Nothing.

Breaking into many substrings
split :: Word8 -> ByteString -> [ByteString]Source#

O(n) Break a ByteString into pieces separated by the byte argument, consuming the delimiter. I.e.

split '\n' "a\nb\nd\ne" == ["a","b","d","e"]
split 'a'  "aXaXaXa"    == ["","X","X","X",""]
split 'x'  "x"          == ["",""]
and

intercalate [c] . split c == id
split == splitWith . (==)
As for all splitting functions in this library, this function does not copy the substrings, it just constructs new ByteStrings that are slices of the original.

splitWith :: (Word8 -> Bool) -> ByteString -> [ByteString]Source#

O(n) Splits a ByteString into components delimited by separators, where the predicate returns True for a separator element. The resulting components do not contain the separators. Two adjacent separators result in an empty component in the output. eg.

splitWith (=='a') "aabbaca" == ["","","bb","c",""]
splitWith (=='a') []        == []
Predicates
isPrefixOf :: ByteString -> ByteString -> BoolSource#

O(n) The isPrefixOf function takes two ByteStrings and returns True if the first is a prefix of the second.

isSuffixOf :: ByteString -> ByteString -> BoolSource#

O(n) The isSuffixOf function takes two ByteStrings and returns True iff the first is a suffix of the second.

The following holds:

isSuffixOf x y == reverse x `isPrefixOf` reverse y
However, the real implemenation uses memcmp to compare the end of the string only, with no reverse required..

isInfixOf :: ByteString -> ByteString -> BoolSource#

Check whether one string is a substring of another. isInfixOf p s is equivalent to not (null (findSubstrings p s)).

Search for arbitrary substrings
breakSubstringSource#

:: ByteString	
String to search for

-> ByteString	
String to search in

-> (ByteString, ByteString)	
Head and tail of string broken at substring

Break a string on a substring, returning a pair of the part of the string prior to the match, and the rest of the string.

The following relationships hold:

break (== c) l == breakSubstring (singleton c) l
and:

findSubstring s l ==
   if null s then Just 0
             else case breakSubstring s l of
                      (x,y) | null y    -> Nothing
                            | otherwise -> Just (length x)
For example, to tokenise a string, dropping delimiters:

tokenise x y = h : if null t then [] else tokenise x (drop (length x) t)
    where (h,t) = breakSubstring x y
To skip to the first occurence of a string:

snd (breakSubstring x y)
To take the parts of a string before a delimiter:

fst (breakSubstring x y)
Note that calling `breakSubstring x` does some preprocessing work, so you should avoid unnecessarily duplicating breakSubstring calls with the same pattern.

findSubstringSource#

:: ByteString	
String to search for.

-> ByteString	
String to seach in.

-> Maybe Int	 
Deprecated: findSubstring is deprecated in favour of breakSubstring.

Get the first index of a substring in another string, or Nothing if the string is not found. findSubstring p s is equivalent to listToMaybe (findSubstrings p s).

findSubstringsSource#

:: ByteString	
String to search for.

-> ByteString	
String to seach in.

-> [Int]	 
Deprecated: findSubstrings is deprecated in favour of breakSubstring.

Find the indexes of all (possibly overlapping) occurances of a substring in a string.

Searching ByteStrings
Searching by equality
elem :: Word8 -> ByteString -> BoolSource#

O(n) elem is the ByteString membership predicate.

notElem :: Word8 -> ByteString -> BoolSource#

O(n) notElem is the inverse of elem

Searching with a predicate
find :: (Word8 -> Bool) -> ByteString -> Maybe Word8Source#

O(n) The find function takes a predicate and a ByteString, and returns the first element in matching the predicate, or Nothing if there is no such element.

find f p = case findIndex f p of Just n -> Just (p ! n) ; _ -> Nothing
filter :: (Word8 -> Bool) -> ByteString -> ByteStringSource#

O(n) filter, applied to a predicate and a ByteString, returns a ByteString containing those characters that satisfy the predicate.

partition :: (Word8 -> Bool) -> ByteString -> (ByteString, ByteString)Source#

O(n) The partition function takes a predicate a ByteString and returns the pair of ByteStrings with elements which do and do not satisfy the predicate, respectively; i.e.,

partition p bs == (filter p xs, filter (not . p) xs)
Indexing ByteStrings
index :: ByteString -> Int -> Word8Source#

O(1) ByteString index (subscript) operator, starting from 0.

elemIndex :: Word8 -> ByteString -> Maybe IntSource#

O(n) The elemIndex function returns the index of the first element in the given ByteString which is equal to the query element, or Nothing if there is no such element. This implementation uses memchr(3).

elemIndices :: Word8 -> ByteString -> [Int]Source#

O(n) The elemIndices function extends elemIndex, by returning the indices of all elements equal to the query element, in ascending order. This implementation uses memchr(3).

elemIndexEnd :: Word8 -> ByteString -> Maybe IntSource#

O(n) The elemIndexEnd function returns the last index of the element in the given ByteString which is equal to the query element, or Nothing if there is no such element. The following holds:

elemIndexEnd c xs ==
(-) (length xs - 1) `fmap` elemIndex c (reverse xs)
findIndex :: (Word8 -> Bool) -> ByteString -> Maybe IntSource#

The findIndex function takes a predicate and a ByteString and returns the index of the first element in the ByteString satisfying the predicate.

findIndices :: (Word8 -> Bool) -> ByteString -> [Int]Source#

The findIndices function extends findIndex, by returning the indices of all elements satisfying the predicate, in ascending order.

count :: Word8 -> ByteString -> IntSource#

count returns the number of times its argument appears in the ByteString

count = length . elemIndices
But more efficiently than using length on the intermediate list.

Zipping and unzipping ByteStrings
zip :: ByteString -> ByteString -> [(Word8, Word8)]Source#

O(n) zip takes two ByteStrings and returns a list of corresponding pairs of bytes. If one input ByteString is short, excess elements of the longer ByteString are discarded. This is equivalent to a pair of unpack operations.

zipWith :: (Word8 -> Word8 -> a) -> ByteString -> ByteString -> [a]Source#

zipWith generalises zip by zipping with the function given as the first argument, instead of a tupling function. For example, zipWith (+) is applied to two ByteStrings to produce the list of corresponding sums.

unzip :: [(Word8, Word8)] -> (ByteString, ByteString)Source#

O(n) unzip transforms a list of pairs of bytes into a pair of ByteStrings. Note that this performs two pack operations.

Ordered ByteStrings
sort :: ByteString -> ByteStringSource#

O(n) Sort a ByteString efficiently, using counting sort.

Low level conversions
Copying ByteStrings
copy :: ByteString -> ByteStringSource#

O(n) Make a copy of the ByteString with its own storage. This is mainly useful to allow the rest of the data pointed to by the ByteString to be garbage collected, for example if a large string has been read in, and only a small part of it is needed in the rest of the program.

Packing CStrings and pointers
packCString :: CString -> IO ByteStringSource#

O(n). Construct a new ByteString from a CString. The resulting ByteString is an immutable copy of the original CString, and is managed on the Haskell heap. The original CString must be null terminated.

packCStringLen :: CStringLen -> IO ByteStringSource#

O(n). Construct a new ByteString from a CStringLen. The resulting ByteString is an immutable copy of the original CStringLen. The ByteString is a normal Haskell value and will be managed on the Haskell heap.

Using ByteStrings as CStrings
useAsCString :: ByteString -> (CString -> IO a) -> IO aSource#

O(n) construction Use a ByteString with a function requiring a null-terminated CString. The CString is a copy and will be freed automatically; it must not be stored or used after the subcomputation finishes.

useAsCStringLen :: ByteString -> (CStringLen -> IO a) -> IO aSource#

O(n) construction Use a ByteString with a function requiring a CStringLen. As for useAsCString this function makes a copy of the original ByteString. It must not be stored or used after the subcomputation finishes.

I/O with ByteStrings
Standard input and output
getLine :: IO ByteStringSource#

Read a line from stdin.

getContents :: IO ByteStringSource#

getContents. Read stdin strictly. Equivalent to hGetContents stdin The Handle is closed after the contents have been read.

putStr :: ByteString -> IO ()Source#

Write a ByteString to stdout

putStrLn :: ByteString -> IO ()Source#

Deprecated: Use Data.ByteString.Char8.putStrLn instead. (Functions that rely on ASCII encodings belong in Data.ByteString.Char8)

Write a ByteString to stdout, appending a newline byte

interact :: (ByteString -> ByteString) -> IO ()Source#

The interact function takes a function of type ByteString -> ByteString as its argument. The entire input from the standard input device is passed to this function as its argument, and the resulting string is output on the standard output device.

Files
readFile :: FilePath -> IO ByteStringSource#

Read an entire file strictly into a ByteString.

writeFile :: FilePath -> ByteString -> IO ()Source#

Write a ByteString to a file.

appendFile :: FilePath -> ByteString -> IO ()Source#

Append a ByteString to a file.

I/O with Handles
hGetLine :: Handle -> IO ByteStringSource#

Read a line from a handle

hGetContents :: Handle -> IO ByteStringSource#

Read a handle's entire contents strictly into a ByteString.

This function reads chunks at a time, increasing the chunk size on each read. The final string is then realloced to the appropriate size. For files > half of available memory, this may lead to memory exhaustion. Consider using readFile in this case.

The Handle is closed once the contents have been read, or if an exception is thrown.

hGet :: Handle -> Int -> IO ByteStringSource#

Read a ByteString directly from the specified Handle. This is far more efficient than reading the characters into a String and then using pack. First argument is the Handle to read from, and the second is the number of bytes to read. It returns the bytes read, up to n, or empty if EOF has been reached.

hGet is implemented in terms of hGetBuf.

If the handle is a pipe or socket, and the writing end is closed, hGet will behave as if EOF was reached.

hGetSome :: Handle -> Int -> IO ByteStringSource#

Like hGet, except that a shorter ByteString may be returned if there are not enough bytes immediately available to satisfy the whole request. hGetSome only blocks if there is no data available, and EOF has not yet been reached.

hGetNonBlocking :: Handle -> Int -> IO ByteStringSource#

hGetNonBlocking is similar to hGet, except that it will never block waiting for data to become available, instead it returns only whatever data is available. If there is no data available to be read, hGetNonBlocking returns empty.

Note: on Windows and with Haskell implementation other than GHC, this function does not work correctly; it behaves identically to hGet.

hPut :: Handle -> ByteString -> IO ()Source#

Outputs a ByteString to the specified Handle.

hPutNonBlocking :: Handle -> ByteString -> IO ByteStringSource#

Similar to hPut except that it will never block. Instead it returns any tail that did not get written. This tail may be empty in the case that the whole string was written, or the whole original string if nothing was written. Partial writes are also possible.

Note: on Windows and with Haskell implementation other than GHC, this function does not work correctly; it behaves identically to hPut.

hPutStr :: Handle -> ByteString -> IO ()Source#

A synonym for hPut, for compatibility

hPutStrLn :: Handle -> ByteString -> IO ()Source#

Deprecated: Use Data.ByteString.Char8.hPutStrLn instead. (Functions that rely on ASCII encodings belong in Data.ByteString.Char8)

Write a ByteString to a handle, appending a newline byte

breakByte :: Word8 -> ByteString -> (ByteString, ByteString)Source#

Deprecated: It is an internal function and should never have been exported. Use 'break (== x)' instead. (There are rewrite rules that handle this special case of break.)

breakByte breaks its ByteString argument at the first occurence of the specified byte. It is more efficient than break as it is implemented with memchr(3). I.e.

break (=='c') "abcd" == breakByte 'c' "abcd"
```