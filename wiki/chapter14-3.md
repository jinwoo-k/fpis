### 14.3 순수성은 문맥에 의존한다.
* 특정 범위를 벗어나지 않기 때문에 외부에서는 관측되지 않는 효과,  
  즉 자료가 변이 되더라도 그 변이에 대한 참조가 없는 곳에서는 그 자료의 변이를 알아채지 못한다.
* 앞에서의 상황과는 다르게 관측이 가능할 수도 아닐 수도 있는 효과들도 존재 한다.
```bash
scala> case class Foo(s: String)

scala> val x = Foo("hello")
x: Foo = Foo(hello)

scala> val d = x eq x
d: Boolean = true

scala> val c = Foo("hello") eq Foo("hello")
c: Boolean = false

```
> eq : **참조 상등**(reference equality: java에서 물려 받은 개념)

* 기존의 참조 투명성의 정의에 의하면 `스칼라에서 모든 자료 생성자는 부수효과가 있다.`
* 좀 더 일반적인 참조 투명성 정의

```text
만일 어떤 프로그램에p에서 표현식 e의 모든 출현(occurrence)을 e의 평가 결과로 치한해도 p의 의미에 아무런 영향을 미치치 않는다면, 그 프로그램 p에 관해서 표현식 e는 **참조투명**하다(referentially transparent).

e의 효과가 p에 관한 e의 참조 투명성에 영향을 미치지 않는다면, 프로그램 p의 문맥에서 이것은 부수효과가 아니라고 할 수 있다.
```

#### 14.3.1 부수효과로 간주되는 것은 무엇인가?
```scala
def timesTwo(x: Int) = {
  if (x < 0) println("Got a negative number")
  x * 2
}
```
* 부수 효과가 존재하지만, 프로그램의 다른 어떤 부분이 timesTwo 내부에서 발생하는 println(표준 입출력) 부수효과를 관측할 가능성이 낮다.
* 경우에 따라서 프로그램 문맥상 중요한 출력일 경우 IO 모나드를 이용해 해당 호출을 추적할 수 있다.
* 효과의 추적은 프로그래머로서 우리가 결정하는 선택이다. 이는 가치 평가이며, 선택에는 항상 절충(trade-off)이 따른다.
* 프로그램의 정확성이 의존하는 효과들을 추적하는 것을 권장

### 14.4 요약
* 자료의 변이가 지역 범위를 벗어나지 않는다면 문제가 되지 않음. 내부적으로 지역상태를 변이하면서도 순수한 인터페이스를 작성할 수 있다.(스칼라의 형식 시스템으로 보장 가능)
* 함수의 순수성은 문맥에 의존한다. 프로그래머 또는 언어 설계자가 프로그램의 문맥에 어긋나지 않게 효과를 내포할 수 있다.