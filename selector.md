# css3

하나의 요소를 가리키는 여러 개의 선택자가 존재하는 경우 우선 순위가 높은 선택자는 우선 순위가 낮은 선택자의 속성값을 덮어 쓸 수 있다.

# rule 1. 나중에 선언한 코드가 우선순위가 높다.

---

    .a{color:red;}//우선순위 낮음
    .b{color:blue;}//우선순위 높음
    
    <i class="a b">blue로 노출됩니다.</i>

삽입 스타일과 외부 스타일 또한 이 규칙의 적용을 받는다. <style>과 <link> 중 나중에 선언한 코드의 우선순위가 더 높다.

    //삽입 스타일
    <style>
    	.a{color:red;}//우선순위 낮음
    </style>
    
    //외부 스타일
    <link rel="stylesheet" href="main.css">
    	//main.css
    	. b{clolr:blue;}//우선순위 높음
    
    <i class="a b">blue로 노출됩니다.</i>

삽입 스타일은 외부 스타일보다 항상 나중에 선언 하는 것을 강력하게 권장한다.

# rule 2. rule2는 rule1보다 우선 순위가 높다.

---

    1. 인라인 스타일 //구체성이 가장 높다.
    2. 아이디 선택자 //구체성이 두 번째 높다.
    3. 클래스, 가상 클래스, 속성 선택자 //구체성이 세 번째 높다.
    4. 타입, 가상요소 선택자 //구체성이 네번째 높다.

우선순위를 고려하여 나중에 선언한 선택자라도, 구체성 점수가 낮으면 앞서 작성한 선택자보다 우선 순위에서 밀리게 된다.

선택자 구체성 점소를 계산하는 공식은 다음과 같다.

선택자에서 구체성을 발견할 때 마다 아래 칸에 1을 더한다.

|0|0(a)|0(b)|0(c)|
|------|---|---|---|
|인라인 스타일|아이디 선택자(#test)|클래스(.test),<br/>가상 클래스(:checked,:disabled,:target,:not ...),<br/>속성 선택자([attribute],[attribute="value"]...)<br/>|타입(h1,p,div,span...),<br/>가상 요소 선택자(::before,::after)|


특정 선택자의 구체성 점수가 1000이라면 구체성 점수가 0000~0999인 선택자 보다 우선 순위가 높다.

    *               /* a=0 b=0 c=0 -> specificity =   0 */
    LI              /* a=0 b=0 c=1 {타입(LI)} -> specificity =   1 */
    UL LI           /* a=0 b=0 c=2 {타입(UL) + 타입(LI)} -> specificity =   2 */
    UL OL+LI        /* a=0 b=0 c=3 {타입(UL) + 타입(OL) + 타입(LI)}-> specificity =   3 */
    H1 + *[REL=up]  /* a=0 b=1{속성 선택자([REL=up])} c=1{타입(H1)} -> specificity =  11 */
    UL OL LI.red    /* a=0 b=1{클래스(.red)} c=3{타입(UL) + 타입(OL) + 타입(LI)} -> specificity =  13 */
    LI.red.level    /* a=0 b=2{클래스(.red) + 클래스(.level)} c=1{타입(LI)}-> specificity =  21 */
    #x34y           /* a=1{아이디(#x34y)} b=0 c=0 -> specificity = 100 */
    #s12:not(FOO)   /* a=1{아이디(#s12)} b=0 c=1{부정 가상 클래스(:not())} -> specificity = 101 */

부정 가상 클래스{:not(X)}는 다른 요소들과 마찬가지로 점수가 매겨 지지만, 부정 가상 클래스 자체는 가상 클래스로서 점수가 매겨지지는 않습니다.

This guide is based on 

- [https://github.com/feross/standard](https://www.w3.org/TR/selectors-3/#specificity)
- [https://www.slideshare.net/wsconf/css-selectors-in-html-wsconf-seoul-2016/1](https://www.slideshare.net/wsconf/css-selectors-in-html-wsconf-seoul-2016/1)
