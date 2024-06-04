> [Haskell Snippets](../README.md) / [01. Haskell 시작하기](README.md) / Stack ( 가상환경 ).md
## Stack ( 가상환경 )
Stack 홈페이지 : [https://docs.haskellstack.org/en/stable/README/](https://docs.haskellstack.org/en/stable/README/)

Stack Tutorial : [https://github.com/Originate/guide/blob/master/haskell/stack-tutorial.md](https://github.com/Originate/guide/blob/master/haskell/stack-tutorial.md)

Stackage : [https://www.stackage.org/](https://www.stackage.org/)

요즘의 프로그래밍  언어라면 가상환경이라는 부분에 대해 지원을 하는 편이다.

#### Start your new project:

```
stack new my-project
cd my-project
stack setup
stack build
stack exec my-project-exe
```

#### 특정 버전의 하스켈 설치하기
버전목록 : [https://github.com/commercialhaskell/lts-haskell#readme](https://github.com/commercialhaskell/lts-haskell#readme)
```
stack new project_name --resolver lts-8.21
```

#### 특정 버전의 GHC로 프로젝트 초기화하기
```
stack init --force --resolver lts-12.26
```

#### Compile
```
stack build
```

#### Run Test

run all of test-suit

```
stack test
```

run specify test-suit

```
stack test :et-test
```

#### Run Benchmark

run all of benchmarks

```
stack bench
```

run specify benchmark

```
stack bench :et-bench-misc
```

