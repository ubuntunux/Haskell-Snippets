[http://hackage.haskell.org/package/dlist-0.8.0.7/docs/Data-DList.html](http://hackage.haskell.org/package/dlist-0.8.0.7/docs/Data-DList.html)

[http://h2.jaguarpaw.co.uk/posts/demystifying-dlist/](http://h2.jaguarpaw.co.uk/posts/demystifying-dlist/)

[https://ocharles.org.uk/blog/posts/2012-12-14-24-days-of-hackage-dlist.html](https://ocharles.org.uk/blog/posts/2012-12-14-24-days-of-hackage-dlist.html)

Today we look at a single module that clocks in at a mere 200 lines of code, including comments. In these few lines though is an optimisation that has a wide range of use cases - an optimisation taking an operation from O(n) to O(1). Now that’s my kinda optimisation!

The dlist library provides an API that allows list appending to run in O(1) rather than O(n) time. Before we look at the magic that makes this unbelievable feat possible, let’s take a quick look at how the API works.

```
> toList $ append (fromList [1..5]) (fromList [1..10])
[1,2,3,4,5,1,2,3,4,5,6,7,8,9,10]
```

That’s almost all there is to it. Dlist simply wraps this up in a new type and provides instances for Monad, Monoid, and a few other type classes we’d expect. It should that dlist doesn’t make everything O(1) - at some point you will have to construct the list, which will be at least O(n). However, for specific usage patterns dlist can give you a good boost in performance.

```
Construction
fromList :: [a] -> DList aSource#

Convert a list to a dlist

toList :: DList a -> [a]Source#

Convert a dlist to a list

apply :: DList a -> [a] -> [a]Source#

Apply a dlist to a list to get the underlying list with an extension

apply (fromList xs) ys = xs ++ ys
Basic functions
empty :: DList aSource#

Create a dlist containing no elements

singleton :: a -> DList aSource#

Create dlist with a single element

cons :: a -> DList a -> DList a infixr 9Source#

O(1). Prepend a single element to a dlist

snoc :: DList a -> a -> DList a infixl 9Source#

O(1). Append a single element to a dlist

append :: DList a -> DList a -> DList aSource#

O(1). Append dlists

concat :: [DList a] -> DList aSource#

O(spine). Concatenate dlists

replicate :: Int -> a -> DList aSource#

O(n). Create a dlist of the given number of elements

list :: b -> (a -> DList a -> b) -> DList a -> bSource#

O(n). List elimination for dlists

head :: DList a -> aSource#

O(n). Return the head of the dlist

tail :: DList a -> DList aSource#

O(n). Return the tail of the dlist

unfoldr :: (b -> Maybe (a, b)) -> b -> DList aSource#

O(n). Unfoldr for dlists

foldr :: (a -> b -> b) -> b -> DList a -> bSource#

O(n). Foldr over difference lists

map :: (a -> b) -> DList a -> DList bSource#

O(n). Map over difference lists.
```
