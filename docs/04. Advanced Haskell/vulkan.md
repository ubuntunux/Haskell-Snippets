#### Vulkan SDK 설치

[https://vulkan.lunarg.com/](https://vulkan.lunarg.com/)


#### Vulkan SDK 의존성 패키지 설치

```
sudo apt-get install libglm-dev cmake libxcb-dri3-0 libxcb-present0 libpciaccess0 libpng-dev libxcb-keysyms1-dev libxcb-dri3-dev libx11-dev libmirclient-dev libwayland-dev libxrandr-dev libxcb-ewmh-dev
```

#### Vulkan  SDK 환경 설정
```
export VULKAN_SDK=/home/ubuntunux/vulkan/1.1.85.0/x86_64
export PATH=$VULKAN_SDK/bin:$PATH
export LD_LIBRARY_PATH=$VULKAN_SDK/lib:$LD_LIBRARY_PATH
export VK_LAYER_PATH=$VULKAN_SDK/etc/explicit_layer.d
```


#### Haskell Vulkan-API 라이브러리 설치

[https://github.com/achirkin/vulkan](https://github.com/achirkin/vulkan)

일단 vulkan-api 코드 생성

```
git clone --recursive https://github.com/achirkin/vulkan
cd vulkan

cd genvulkan
stack build
stack exec genvulkan
```

Windows인 경우는 GLFW 설치 ( http://www.glfw.org/ )


#### vulkan 예제 코드 build
vulkan-examples폴더로 이동후 stack으로 빌드후 삼각형 그리기 예제 실행
```
cd vulkan-examples
stack build
stack exec ve-06-Drawing
```

#### Trouble Shooting

Ubuntu에서 빌드중에 아래와 같이 Missing C libraries 에러가 난다면 내용 그대로 c library가 설치가 안된 경우이거나 라이브러리 경로를 설정해줘야 한다.

###### vulkan-1 라이브러리의 경로가 설정이 안된 경우
```
 Configuring vulkan-api-1.3.0.0...
    Cabal-simple_Z6RU0evB_2.2.0.1_ghc-8.4.4.exe: Missing dependency on a foreign
    library:
    * Missing (or bad) C library: vulkan-1
    This problem can usually be solved by installing the system package that
    provides this library (you may need the "-dev" version). If the library is
    already installed but in a non-standard location then you can use the flags
    --extra-include-dirs= and --extra-lib-dirs= to specify where it is.If the
    library file does exist, it may contain errors that are caught by the C
    compiler at the preprocessing stage. In this case you can re-run configure
    with the verbosity flag -v3 to see the error messages.
```
vulkan-1 라이브러리를 못찾는 겨우 두가지 방법이 있다.

방법1) stack.yaml파일을 열고 extra-include-dirs, extra-lib-dirs 경로를 추가하기. 참고로 전역변수 처럼 설정하고 싶다면 stack이 설치돈 경로에 config.yaml 파일을 열고 그곳에 추가해도 된다.
```
extra-include-dirs:
  - C:\VulkanSDK\1.1.130.0\Include

extra-lib-dirs:
  - C:\VulkanSDK\1.1.130.0\Lib
```
방법2) stack.yaml을 열고 useNativeFFI값을 false로 하는 방법.
```
flags:
  vulkan-api:
    useNativeFFI-1-0: false
```



##### Xi, Xcursor, Xinerama 라이브러리가 설치가 안된경우
```
Process exited with code: ExitFailure 1
    Logs have been written to: /home/ubuntunux/Work/vulkan/vulkan-examples/.stack-work/logs/bindings-GLFW-3.2.1.1.log

    Configuring bindings-GLFW-3.2.1.1...
    Cabal-simple_mPHDZzAJ_1.24.2.0_ghc-8.0.2: Missing dependencies on foreign
    libraries:
    * Missing C libraries: Xi, Xcursor, Xinerama
    This problem can usually be solved by installing the system packages that
    provide these libraries (you may need the "-dev" versions). If the libraries
    are already installed but in a non-standard location then you can use the
    flags --extra-include-dirs= and --extra-lib-dirs= to specify where they are.
```

명령으로 직접 설치해주고 stack build를 다시 실행하자

```
sudo apt-get install libx11-dev libxcursor-dev libxinerama-dev 
```