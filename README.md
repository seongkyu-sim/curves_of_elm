
Elm `ver0.18`
---

##### [elm](http://elm-lang.org) 학습 기록

_전체내용을 다루지 않음, 공식문서중 이해하기 어렵거나 혼동하기 쉬운 부분만 먼저 기록합니다._


공식문서
---
1. [튜토리얼](https://www.elm-tutorial.org/en/) 
2. [가이드](https://guide.elm-lang.org)

_Functional Programming에대한 사전 지식이 부족하면 [튜토리얼](https://www.elm-tutorial.org/en/) 을 먼저 읽기 권함_


참고자료
---

1. [Why You Should Give Elm a Try](http://devnacho.com/2016/04/12/why-you-should-give-elm-a-try/) : 왜 Elm을 써야하는가에대한 간략한 설명과 3개의 유투브 영상이 링크되어 있음
2. [케빈TV](https://www.youtube.com/playlist?list=PLRIMoAKN8c6NRxXgxZVo1Jyxgg4TVIKeI) : '나는 프로그래머다' 엠씨로 활동중이신 개발자분, 중간중간 잡담이 있지만 같이 공부하는 기분이 들게함 :) `강추!`


todos
---

- [x] 개념잡기
- [ ] [튜토리얼](https://www.elm-tutorial.org/en/) 
- [ ] [가이드](https://guide.elm-lang.org)
- [ ] Elm + Firebase 테스트


구조
---

언어자체에 웹앱을 만들수 있는 프레임웍이 내장되어 있음
model, view, update가 작동하는 방식이 프레임웍에 의존해서 처리됨
<!--
https://www.elm-tutorial.org/en/02-elm-arch/04-flow.png 설명 쓰기
-->

<!--
###함수 우선권 (Functional programming)
todo: fill it
-->

functions
---

>오브젝트나 값만을 주고 받는게 아니라 함수(처리 알고리즘)을 주고-받음(`λ : 람다`)으로서 구조를 간결하고 유연하게 만든다


```Elm
-- ver. Anonymous functions
\ a b -> a / b
> (\ a b -> a / b) 3 2 -- 이름이 없는 함수 일때는 함수가 어디서 끝나는지 알려주기 위해 '()'로 감싼다
1.5 : Float

-- ver. Named functions
> divide : Float -> Float -> Float
> divide a b= a / b
<function> : Float -> Float -> Float
> divide 3 2
1.5 : Float

-- 'divide 3' 하나의 파라미터만 보내면 어떠한 결과가 나올까?
> divide 3
<function> : Float -> Float -- 함수를 반환함, 함수를 변수에 넣어보자
> divdeThreeBy = divide 3
<function> : Float -> Float
> divideThreeBy 2
1.5 : Float
> divideThreeBy 3
1 : Float

-- ver. Actual
> divide x y = x / y
<function> : Float -> Float -> Float

> divide x = \y -> x / y
<function> : Float -> Float -> Float

> divide = \x -> (\y -> x / y)
<function> : Float -> Float -> Float
```

- 실제와 좀 다르지만 이해하기 쉬운 설명 : __마지막 -> Float__ 부분이 리턴.
- 좀더 어려운 설명 : 'divide : Float -> Float -> Float'은 실제로는 두개의 함수로 이루어져 있으며 __맨뒤의 함수부터 앞쪽의 함수에 함수 자체를 반환__ 시켜서 연산을 함.



record 생성방법
---

```Elm
type alias Animal = 
	{ species : String
	, age : Int
	}
m = { species = "dog", age = 4 } -- 새로운 record를 생성한다
m2 = { m | age = 2 } -- m record를 카피해서 age 값만 변경한다
m3 = Animal "cat" 3 -- 새로운 record를 간소화한 문법으로 생성한다
```



Union Types 
---

### Algebraic data type

[DOC](https://guide.elm-lang.org/types/union_types.html)

OOP개념의 `enum`과 유사한 개념, 단순히 타입뿐아니라 각 타입함수가 값을 받을 수 있다. 



```Elm
type User = Anonymous | Named String


userPhoto : User -> String
userPhoto user =
  case user of
    Anonymous ->
      "anon.png"

    Named name ->
      "users/" ++ name ++ ".png"


activeUsers : List User
activeUsers =
  [ Anonymous, Named "catface420", Named "AzureDiamond", Anonymous ]


photos : List String
photos =
  List.map userPhoto activeUsers
-- [ "anon.png", "users/catface420.png", "users/AzureDiamond.png", "anon.png" ]
```

<!--[Swift와 비교](https://github.com/seongkyu-sim/curves_of_elm/blob/master/compareWithSwift.md)-->
[Swift와 비교](https://github.com/seongkyu-sim/curves_of_elm/blob/master/compareWithSwift.md#enum--associated-dataalgebraic-data-type)


### Generic Data Structures

> 데이터의 형태를 정하지 않고 처리하기위한 패턴

```elm
> type List a = Empty | Node a (List a)

> ns = Node 1 (Node 2 (Node 3 Empty))
-- Node 1 (Node 2 (Node 3 Empty)) : Repl.List number
> nil = Empty
-- Empty : Repl.List a


> isEmpty list = \
|   case list of\
|     Empty -> True\
|     Node _ _ -> False
-- <function> : Repl.List a -> Bool

> isEmpty ns
-- False : Bool
> isEmpty nil
-- True : Bool


> length list = \
|   case list of \
|     Empty -> 0 \
|     Node _ next -> 1 + length next
<function> : Repl.List a -> number

> length nil
-- 0 : number

> length ns
-- 3 : number


> reverse list = \
|   let \
|     reverseP xs acc = \
|       case xs of \
|         Empty -> acc \
|         Node a next -> reverseP next (Node a acc) \
|   in \
|     reverseP list Empty
-- <function> : Repl.List a -> Repl.List a

> reverse nil
-- Empty : Repl.List a
> reverse ns
-- Node 3 (Node 2 (Node 1 Empty)) : Repl.List number


> member a list = \
|   case list of \
|     Empty -> False \
|     Node x next -> if a == x then True else member a next 
-- <function> : a -> Repl.List a -> Bool

> member 2 ns
-- True : Bool
> member 4 ns
-- False : Bool
```

- `_`(underscore) 해당 값을 사용하지않음(무시)
- List `a`: a를 써도 되고 어떤 String값이든 쓸수 있음 단 컨벤션은 lowercase로 시작.
- type 생성시 위 예제에서 List가 타입의 역할을 하고 오른쪽의 (Empty, Node a)부분이 데이터 역할을 하게됨.


Error Handling
---

- Maybe: 타언어(Java, Javascript, Ruby, Python)에 있는 null에 해당하는 상황을 처리하기위해 만들어짐. Swift의 `optional`과 비슷한 개념
- Result: exeption을 처리하기 위해 만들어짐.
- Task: Result와 비슷하나 `asynchronos`상황에 특화되어 만들어짐.

### Maybe
```Elm
> type Maybe a = Nothing | Just a

> Nothing
Nothing : Repl.Maybe a
> Just
<function> : a -> Repl.Maybe a
> Just 3
Just 3 : Repl.Maybe number
> Just "frank"
Just "frank" : Repl.Maybe String -- 아마 스트링이 있을걸
```


### Optional Fields

옵셔널이 필요한이유: 유저에게 정보입력 요구를 분할하라. ex)처음가입할때 이메일만 요구하고 사용하면서 이름, 성별등의 정보를 분할 요청하라 --> UX관점에서도 중요한 포인트

```Elm
type alias User =
  { name : String
  , age : Maybe Int
  }  

sue : User
sue =
  { name = "Sue", age = Nothing }  

tom : User
tom =
  { name = "Tom", age = Just 24 }


canBuyAlcohol : User -> Bool
canBuyAlcohol user =
  case user.age of -- 옵셔널로 설정된 값을 사용하려면 case문으로 풀어서 사용해야한다.
    Nothing ->
      False

    Just age ->
      age >= 21  
```

- [Swift비교](https://github.com/seongkyu-sim/curves_of_elm/blob/master/compareWithSwift.md#optional): optional 사용은 Swift가 편하다고 느껴짐, 하지만 sue.age! 처럼 강제로 무시가능(nil이 없는 것이 아님)

