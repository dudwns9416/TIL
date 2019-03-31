클래스
=======
```kotlin
    class invoice{

    }   
```
기본생성자
------
constructor는 생략 가능.
초기화는 init 블록 안에서 작성.
```kotlin
    class Person constructor(firstName: String) {
        init {
            logger.info("Person initialized with value ${firstName}")
        }
    }
```
기본생성자에 어노테이션 접근지정자 등이 있는 경우에는 constructor 키워드가 필요하다.

보조생성자
------
```kotlin
    class Person {
        constructor(parent: Person){
            parent.children.add(this)
        }
    }
```
클래스가 기본생성자를 가지고 있다면, 각각의 보조생성자들은 기본생성자를 직접 또는 간접적으로 위임해 주어야 한다.
* 직접적 : 기본생성자로부터 위임을 받는다.
* 간접적 : 다른 보조생성자로부터 위임을 받는다.
```kotlin
    class Person(val name:String) {
         constructor(name:String, parent: Person) : this(name) {

        }
        constructor() : this("홍길동",Person()){

        }
    }
```
* 기본생성자가 private이길 원한다면
```kotlin
class DontCreateMe private constructor (){

}
```

상속
======
Any : 코틀린의 최상위 클래스

class Example1 // 암시적인 Any 상속

    public open class Any {
        public open operatior fun equals(other: Any?): Boolean
        public open fun hashCode(): Int
        public open fun toString(): String
    }

 기본적으로 코틀린의 모든 class은 final이다.
 그래서 상속을 쓰려면 open 어노테이션이 필요하다.

인터페이스

추상 클래스