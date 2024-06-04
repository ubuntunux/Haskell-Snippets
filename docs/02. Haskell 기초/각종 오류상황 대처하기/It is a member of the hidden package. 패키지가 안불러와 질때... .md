패키지를 불러오려고 했더니 아래와 같은 에러메시지를 내뿜는 경우가 있다.
패키지가 숨겨져 있다는 뜻인거 같다.

    *Main Lib> import Data.Time
    
    <no location info>:
        Could not find module ‘Data.Time’
        It is a member of the hidden package ‘time-1.5.0.1@time_IYbjC7tGONY15oDy1fGJKz’.

아래와 같이 ghci를 나간다음 다시 실행할때 옵션을 붙여주면 된다.

`$ stack ghci --no-package-hiding time-1.5.0.1`