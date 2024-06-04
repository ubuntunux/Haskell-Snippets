[https://wiki.haskell.org/HXT](https://wiki.haskell.org/HXT)

[https://github.com/UweSchmidt/hxt](https://github.com/UweSchmidt/hxt)

[https://wiki.haskell.org/HXT/Practical](https://wiki.haskell.org/HXT/Practical)

[http://adit.io/posts/2012-04-14-working_with_HTML_in_haskell.html](http://adit.io/posts/2012-04-14-working_with_HTML_in_haskell.html)

[https://en.wikibooks.org/wiki/Haskell/XML](https://en.wikibooks.org/wiki/Haskell/XML)

```
λ> import Text.XML.HXT.Core
λ> xml <- readFile "Resource/Externals/Meshes/skeletal.dae"
λ> xml
"<?xml version=\"1.0\" encoding=\"utf-8\"?>
   <COLLADA xmlns=\"http://www.collada.org/2005/11/COLLADASchema\" version=\"1.4.1\">
     <emission>
       <color sid=\"emission\">0 0 0 1</color>
     </emission>
     <ambient>
       <color sid=\"ambient\">0 0 0 1</color>
     </ambient>
   </COLLADA>"
   
λ> let doc = readString [withParseHTML yes, withWarnings no] xml
λ> runX doc
[NTree (XTag "/" [NTree (XAttr "source") [NTree (XText "\"<?xml version=\\\"1.0\\\" encoding=\\\"utf-8\\\"?>\\n<COLLA...\"") []],NTree (XAttr "transfer-URI") [NTree (XText "string:") []],NTree (XAttr "transfer-Message") [NTree (XText "OK") []],NTree (XAttr "transfer-Status") [NTree (XText "200") []]]) [NTree (XTag "collada" [NTree (XAttr "xmlns") [NTree (XText "http://www.collada.org/2005/11/COLLADASchema") []],NTree (XAttr "version") [NTree (XText "1.4.1") []]]) [NTree (XText "\n  ") [],NTree (XTag "emission" []) [NTree (XText "\n    ") [],NTree (XTag "color" [NTree (XAttr "sid") [NTree (XText "emission") []]]) [NTree (XText "0 0 0 1") []],NTree (XText "\n  ") []],NTree (XText "\n  ") [],NTree (XTag "ambient" []) [NTree (XText "\n    ") [],NTree (XTag "color" [NTree (XAttr "sid") [NTree (XText "ambient") []]]) [NTree (XText "0 0 0 1") []],NTree (XText "\n  ") []],NTree (XText "\n") []]]]

λ> runX $ doc //> hasName "color"
[NTree (XTag "color" [NTree (XAttr "sid") [NTree (XText "emission") []]]) [NTree (XText "0 0 0 1") []],NTree (XTag "color" [NTree (XAttr "sid") [NTree (XText "ambient") []]]) [NTree (XText "0 0 0 1") []]]

λ> runX $ doc //> hasName "color" >>> getAttrl
[NTree (XAttr "sid") [NTree (XText "emission") []],NTree (XAttr "sid") [NTree (XText "ambient") []]]

λ> runX $ doc //> hasName "color" >>> getAttrValue "sid"
["emission","ambient"]

λ> runX $ doc //> hasName "color" >>> getChildren >>> getText
["0 0 0 1","0 0 0 1"]

```