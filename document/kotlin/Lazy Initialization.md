# kotlin Lazy Initialliztion

지연 초기화를 하면 어떤 점이 좋나요?
클래스가 초기화될 때 모든 것들을 동시에 초기화 하는 코드와 필요한 순간까지 초기화를 최대한 미루는 코드 중 어떤 것이 성능 측면에서 좋을지 생각해보면 답은 쉽게 나온다.

## lateinit
프러퍼티 선언에 사용되며 몇 가지 제약사항이 있다.
* var(mutable) 프러퍼티만 사용 가능
* non-null 프러퍼티만 사용 가능
* 커스텀 getter/setter가 없는 프러퍼티만 사용 가능
* primitive type 프러퍼티는 불가능
* 클래스 생성자에서 사용 불가능
* 로컬 변수로 사용 불가능

lateinit은 '이 프러퍼티'는 
절대로 Null이 될 수 없는 프러퍼티인데 1.초기화를 선언과 동시에 해줄 수 없을 때
2.성능이나 기타 다른 조건들로 인해 최대한 초기화를 미뤄야 할 때
사용한다.

## lazy
람다를 파라미터로 받고 Lazy<T>인스턴스를 반환하는 함수이다. 제약사항은 아래와 같다.
* val(immutable) 프러퍼티만 사용 가능
* primitive type 사용 가능
* 커스텀 getter/setter가 없는 프러퍼티만 사용 가능
* Non-null, Nullable 둘 다 사용가능
* 클래스 생성자에서 사용 불가능
* 로컬 변수에서 사용 가능

초기화 코드를 빼먹는 경우를 방지하기 위해 lazy를 사용하는 것이 적절하다.
    private val toolbar by lazy { home_toolbar }
 lazy 블록 안에서 선언과 초기화가 함께 있다.
 
 lateinit이 Kotlin언어에서 제공하는 기본 Modifier라면 lazy는 Kotlin에서 제공하는 유틸 함수다. 그렇기 때문에 앞으로 변경 가능성도 있고 기능이 추가될 여지도 있다.
 
 게다가 lazy프러퍼티연산은 기본적으로  동기화된다.
```kotlin
    @kotlin.jvm.JvmVersion
    public fun <T> lazy(initializer: () -> T): Lazy<T> = SynchronizedLazyImpl(initializer)

    @kotlin.jvm.JvmVersion
    public fun <T> lazy(mode: LazyThreadSafetyMode, initializer: () -> T): Lazy<T> =
        when (mode) {
            LazyThreadSafetyMode.SYNCHRONIZED -> SynchronizedLazyImpl(initializer)
            LazyThreadSafetyMode.PUBLICATION -> SafePublicationLazyImpl(initializer)
            LazyThreadSafetyMode.None -> UnsafeLazyImpl(initializer)
        }
```
SYNCHRONIZED
한 스레드만 값을 계산할 수 있고 모든 스레드가 같은 값을 보게 된다.

PUBLICATION
여러 스레드에서 write는 가능하더라도 read는 처음 생성된 인스턴스만 반환

NONE
동기화 관련 처리가 없다. 단일 스레드 환경이라면 이 옵션을 사용하는 것이 좋다.


참조
https://medium.com/@joongwon/kotlin-kotlin-lazy-initialization-901079296e43