[https://en.wikibooks.org/wiki/Haskell/Lenses_and_functional_references](https://en.wikibooks.org/wiki/Haskell/Lenses_and_functional_references)

[https://github.com/ekmett/lens](https://github.com/ekmett/lens)

[https://github.com/ekmett/lens/wiki/Examples](https://github.com/ekmett/lens/wiki/Examples)

[https://artyom.me/lens-over-tea-1](https://artyom.me/lens-over-tea-1)

[https://tech.fpcomplete.com/haskell/tutorial/lens](https://tech.fpcomplete.com/haskell/tutorial/lens)

[https://hackage.haskell.org/package/lens-tutorial-1.0.4/docs/Control-Lens-Tutorial.html](https://hackage.haskell.org/package/lens-tutorial-1.0.4/docs/Control-Lens-Tutorial.html)

[https://mmhaskell.com/blog/2017/6/12/taking-a-close-look-at-lenses](https://mmhaskell.com/blog/2017/6/12/taking-a-close-look-at-lenses)


```
{-# LANGUAGE TemplateHaskell #-}

import Control.Lens

data Point = Point { _x :: Double, _y :: Double } deriving (Show)
data Atom = Atom { _element :: String, _point :: Point } deriving (Show)
data Molecule = Molecule { _atoms :: [Atom] } deriving (Show)

$(makeLenses ''Point)
$(makeLenses ''Atom)
$(makeLenses ''Molecule)

shiftAtomX :: Atom -> Atom
shiftAtomX = over (point . x) (+ 1)

shiftMoleculeX :: Molecule -> Molecule
shiftMoleculeX = over (atoms . traverse . point . x) (+ 1)
```

```
>>> let atom = Atom { _element = "C", _point = Point { _x = 1.0, _y = 2.0 } }
>>> shiftAtomX atom
Atom {_element = "C", _point = Point {_x = 2.0, _y = 2.0}}

>>> let atom1 = Atom { _element = "C", _point = Point { _x = 1.0, _y = 2.0 } }
>>> let atom2 = Atom { _element = "O", _point = Point { _x = 3.0, _y = 4.0 } }
>>> let molecule = Molecule { _atoms = [atom1, atom2] }
>>> shiftMoleculeX molecule  -- Output formatted for clarity
Molecule {_atoms = [Atom {_element = "C", _point = Point {_x = 2.0, _y = 2.0}},Atom {_element = "O", _point = Point {_x = 4.0, _y = 4.0}}]}

>>> let atom = Atom { _element = "C", _point = Point { _x = 1.0, _y = 2.0 } }
>>> view (point . x) atom
1.0
>>> atom^.point.x
1.0

>>> set (point . x) 9 atom
Atom {_element = "C", _point = Point {_x = 9.0, _y = 2.0}}
>>> atom & (point . x) .~ 9
Atom {_element = "C", _point = Point {_x = 9.0, _y = 2.0}}
>>> atom^.point & x .~ 9
Point {_x = 9.0, _y = 2.0}

λ> over x (+99.0) $ atom^.point
Point {_x = 100.0, _y = 2.0}
λ> atom^.point & x %~ (+99.0)
Point {_x = 100.0, _y = 2.0}


>>> over (point.x) (+99.0) atom
Atom {_element = "C", _point = Point {_x = 100.0, _y = 2.0}}
>>> atom & point.x %~ (+99.0)
Atom {_element = "C", _point = Point {_x = 100.0, _y = 2.0}}


>>> over mapped negate [1..4]
[-1,-2,-3,-4]
>>> over mapped negate (Just 3)
Just (-3)

>>> _1 (\x -> [0..x]) (4, 1) -- Traversal
[(0,1),(1,1),(2,1),(3,1),(4,1)]
>>> set _1 7 (4, 1) -- Setter 
(7,1)
>>> over _1 length ("orange", 1) -- Setter, changing the types
(6,1)
>>> toListOf _1 (4, 1) -- Fold
[4]
>>> view _1 (4, 1) -- Getter
4
```