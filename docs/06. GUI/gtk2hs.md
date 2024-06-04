> [Haskell Snippets](../README.md) / [06. GUI](README.md) / gtk2hs.md
## gtk2hs
[https://github.com/gtk2hs/gtk2hs](https://github.com/gtk2hs/gtk2hs)

[http://www.muitovar.com/gtk2hs/chap2.html](http://www.muitovar.com/gtk2hs/chap2.html)


##### Install GTK Library
- Ubuntu
```
sudo apt-get install libgirepository1.0-dev libwebkitgtk-3.0-dev libwebkit2gtk-4.0-dev libgtksourceview-3.0-dev
sudo apt-get install libcanberra-gtk-module libcanberra-gtk3-module
```

- Windows MSYS2
Install [MSYS2 ](https://msys2.github.io/)and [Chocolatey](https://chocolatey.org/). Then in a shell with administrator privileges:

```
choco install ghc
pacman -S mingw64/mingw-w64-x86_64-pkg-config mingw64/mingw-w64-x86_64-gobject-introspection mingw64/mingw-w64-x86_64-gtksourceview3 mingw64/mingw-w64-x86_64-webkitgtk3
```


#### GTK Test

```
stack new gtk-test --resolver=lts-12.26
cd gtk-test
```

file open 'gtk-test.cabal' and insert 'gtk' library.

example)
```
library
  build-depends:
      gtk
```

initialize project
```
stack init
stack setup
stack build
```

