Elm `ver0.18`
===

#####[elm](http://elm-lang.org) 학습 기록

_전체내용을 다루지 않음, 공식문서중 이해하기 어렵거나 혼동하기 쉬운 부분만 먼저 기록합니다._

---



###공식문서
1. [튜토리얼](https://www.elm-tutorial.org/en/) 
2. [가이드](https://guide.elm-lang.org)

_Functional Programming에대한 사전 지식이 부족하면 [튜토리얼](https://www.elm-tutorial.org/en/) 을 먼저 읽기 권함_


###참고자료

1. [Why You Should Give Elm a Try](http://devnacho.com/2016/04/12/why-you-should-give-elm-a-try/) : 왜 Elm을 써야하는가에대한 간략한 설명과 3개의 유투브 영상이 링크되어 있음
2. [케빈TV](https://www.youtube.com/playlist?list=PLRIMoAKN8c6NRxXgxZVo1Jyxgg4TVIKeI) : '나는 프로그래머다' 엠씨로 활동중이신 개발자분, 중간중간 잡담이 많치만 같이 공부하는 기분이 들게함 :) __강추!__


---


###todos

- [x] 개념잡기
- [ ] [튜토리얼](https://www.elm-tutorial.org/en/) 
- [ ] [가이드](https://guide.elm-lang.org)
- [ ] Elm + Firebase 테스트



---


###구조
언어자체에 웹앱을 만들수 있는 프레임웍이 내장되어 있음
model, view, update가 작동하는 방식이 프레임웍에 의존해서 처리됨
<!--
https://www.elm-tutorial.org/en/02-elm-arch/04-flow.png 설명 쓰기
-->



<!--
###함수 우선권 (Functional programming)
todo: fill it
-->



###functions

>오브젝이나 값만을 주고 받는게 아니라 함수(처리 알고리즘)을 주고-받음(`λ : 람다`)으로서 구조를 간결하고 유연하게 만든다


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



###record 생성방법

```Elm
type alias Animal = 
	{ species : String
	, age : Int
	}
m = { species = "dog", age = 4 } -- 새로운 record를 생성한다
m2 = { m | age = 2 } -- m record를 카피해서 age 값만 변경한다
m3 = Animal "cat" 3 -- 새로운 record를 간소화한 문법으로 생성한다
```


###Union Types>Algebraic data type

[DOC](https://guide.elm-lang.org/types/union_types.html)


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
