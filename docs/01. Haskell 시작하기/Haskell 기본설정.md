[https://www.youtube.com/watch?v=5p2Aq3bRuL0](https://www.youtube.com/watch?v=5p2Aq3bRuL0)

IntelliJ + Haskell PlugIn is best.


-- 문법적으로 더 나은 방법을 알려줌. warning도 알려줌

stack install hlint

--  파일하나만 hlint

hlint Main.hs

-- 현재 하위 폴더를 전부 hlint

hlint .


--  소스가 수정될때 마다 자동으로 빌드해줌

stack install ghcid

ghcid Main.hs