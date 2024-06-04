> [Haskell Snippets](../README.md) / [05. Haskell Extensions](README.md) / Type Families.md
## Type Families
[https://free.cofree.io/2019/01/08/defunctionalization/](https://free.cofree.io/2019/01/08/defunctionalization/)

[https://wiki.haskell.org/GHC/Type_families](https://wiki.haskell.org/GHC/Type_families)

[https://ocharles.org.uk/posts/2014-12-12-type-families.html](https://ocharles.org.uk/posts/2014-12-12-type-families.html)

[http://www.mchaver.com/posts/2017-06-21-type-families.html](http://www.mchaver.com/posts/2017-06-21-type-families.html)

### Basic Concept
Type-level functions in Haskell are called “type families”. Type families can be defined in a few different ways, such as closed type families and open type families. There is a third way called associated type families, which are type-level functions defined within type classes. It is more commonly used than the first two, but is less relevant to our topic of defunctionalization so we omit it here.

#### Closed Type Families
In a closed type family, the entire type-level function is defined the moment you introduce the type family, under the where clause. For example,

```
import Data.Kind (Constraint, Type)

type family Foo (x :: Type) (y :: Type) :: Type where
  Foo Int Bool = Double
  Foo Int Double = Maybe Char
  Foo String (Maybe a) = Either Int a
  Foo String (Maybe Int) = Maybe String
```

#### Open Type families
In an open type family, the type-level function is defined by type instance declarations which can be extended. For example,

```
type family Baz (x :: Type) (y :: Type) :: Type
type instance Baz Int Bool = Double
type instance Baz Int (Maybe a) = Either
```

타입패밀리의 컨셉은 타입이론으로 부터 출발한다. 타입패밀리가 나타내는 것은 타입레벨에서의 부분함수이며 파라미터에 적용하면 타입을 반환합니다. 타입패밀리는 정적인 단순한 타입시스템에 대한 데이터 생성자 보다는 다형성 타입에 대한 데이터 생성자로써 적합합니다.

일반적인 데이터 타입에 대해 타입패밀리가 어떻게 다르게 동작하는지 엄격한(strict) 리스트 타입에 대해 생각해보자. 우리는 Cons를 이용한 일반적인 방법으로 Char 타입의 리스트를 구현할 수 있다. 또한 같은 방법으로 () 튜플 타입의 리스트를 구현할 수 있지만 () 튜플 타입은 그다지 쓸모있는 정보를 담고 있지 않기 때문에 차라리 리스트의 길이를 저장하는 편이 더 효율적이다. 이런것은 보통의 데이터 타입으로는 할 수 없는 일이다. 만약 타입 파라미터가 Char라면 Cons로 이루어진 리스트일것이고 타입 파라미터가 () 튜플이라면 하나의 정수가 될것이다. 우리는 타입 파라미터에 기반한 두개의 데이터 타입을 선택하길 원한다. 타입패밀리를 사용하여 이런한 리스트를 다음과 같이 선언할수 있다:

일반적으로 XList instance를 선언할 경우 타입변수 정도만 바꿀수 있지만 타입패밀리를 활용하면 단순히 Int값 하나만을 값으로 가지는 XList를 만들수도 있게된다.

#### Data Families
```
-- Declare a list-like data family
data family XList a

-- Declare a list-like instance for Char
data instance XList Char = XCons !Char !(XList Char) | XNil

-- Declare a number-like instance for ()
data instance XList () = XListUnit !Int
```

#### TypeFamilies VS DataFamilies
[https://stackoverflow.com/questions/14195497/data-families-use-cases](https://stackoverflow.com/questions/14195497/data-families-use-cases)

One benefit is that data families are injective, unlike type families.

If you have
```
type family TF a
data family DF a
```
Then you know that DF a ~ DF b implies that a ~ b, while with TF, you don't -- for any a you can be sure that DF a is a completely new type (just like [a] is a different type from [b], unless of course a ~ b), while a type family can map multiple input types onto the same existing type.

A second is that data families can be partially applied, like any other type constructor, while type families can't.

This is not a particularly real-world example, but for example, you can do:
```
data instance DF Int    = DInt    Int
data instance DF String = DString String

class C t where
    foo :: t Int -> t String

instance C DF where -- notice we are using DF without an argument
                    -- notice also that you can write instances for data families at all,
                    -- unlike type families
    foo (DInt i) = DString (show i)
```
Basically, DF and DF a are actual, first-class, legitimate types, in themselves, like any other type you declare with data. TF a is just an intermediate form that evaluates to a type.

But I suppose all of that's not very enlightening, or at least it wasn't for me, when I was wondering about data families and read similar things.

Here's the rule of thumb I go by. Whenever you find yourself repeating the pattern that you have a type family, and for every input type, you declare a new data type for the type family to map onto, it's nicer to cut out the middleman and use a data family instead.

A real-world example from the vector library. vector has several different kinds of Vectors: boxed vectors, unboxed vectors, primitive vectors, storable vectors. For each Vector type there is a corresponding, mutable MVector type (normal Vectors are immutable). So it looks like this:
```
type family Mutable v :: * -> * -> * -- the result type has two type parameters

module Data.Vector{.Mutable} where
data Vector a = ...
data MVector s a = ...
type instance Mutable Vector = MVector

module Data.Vector.Storable{.Mutable} where
data Vector a = ...
data MVector s a = ...
type instance Mutable Vector = MVector
```
[etc.]
Now instead of that, I would rather have:
```
data family Mutable v :: * -> * -> *

module Data.Vector{.Mutable} where
data Vector a = ...
data instance Mutable Vector s a = ...
type MVector = Mutable Vector

module Data.Vector.Storable{.Mutable} where
data Vector a = ...
data instance Mutable Vector s a = ...
type MVector = Mutable Vector
```
[etc.]
Which encodes the invariant that for every Vector type there is exactly one Mutable Vector type, and that there is a one-to-one correspondence between them. The mutable version of a Vector is always called Mutable Vector: that is its name, and it has no other. If you have a Mutable Vector, you can get the type of the corresponding immutable Vector, because it's right there as a type argument. With type family Mutable, once you apply it to an argument it evaluates to an unspecified result type (presumably called MVector, but you can't know), and you have no way to map backwards.