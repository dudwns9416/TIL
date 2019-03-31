## let()
>Calls the specified function block with this value as its argument and returns its result.

``` kotlin
    fun <T, R> T.let(blockL T -> R): R
```

```kotlin
    val padding = TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics).toInt()

    setPadding(padding, 0, padding , 0)
```
여기에서 padding이라는 상수 값이 한 번만 사용되고 더 이상 사용되지 않는다.
이런 경우, 다음과 같이 `let()`을 사용하면 불필요한 선언을 방지한다.
```kotlin
    TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics).toInt().let{
            setPadding(it, 0, it, 0)
        }
```
## apply()
```kotlin
    fun <T> T.apply(block: T.() -> Unit): T
```
함수를 호출하는 객체를 이어지는 블록의 리시버로 전달하고, 객체 자체를 반환한다.
객체를 생성하면서 함께 호출해야 하는 초기화 코드가 있는 경우에 사용할 수 있다.
```kotlin
    val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT)
    param.gravity = Gravity.CENTER_HORIZONTAL
    param.weight = 1f
    param.topMargin = 100
    param.bottomMargin = 100
```
위의 예를 아래와 같이 변경한다.
```kotlin
    val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT).apply {
        gravity = Gravity.CENTER_HORIZONTAL
        weight = 1f
        topMargin = 100
        bottomMargin = 100
    }
```

## run()
인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태 총 두 가지가 있다.
```kotlin
    fun <R> run(block: () -> R): R
```
일반 함수와 마찬가지로 값을 반환하지 않거나 특정 값을 반환할 수 있다.
```kotlin
    fun <T, R> T.run(block: T.() -> R): R
```
`apply()`와 적용 예가 유사하다. 하지만 `apply()`는 객체를 생성함과 동시에 연속된 작업이 필요할 때 사용하고, `run()`은 이미 생성된 객체에 연속된 작업이 필요할 때 사용한다는 점이 다르다.
```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        supportActionBar?.run {
            setDisplayHomeAsUpEnabled(true)
            setHomeAsUpIndicator(R.drawable.ic_clear_white)
        }
        ...
    }
```

## with()
인자로 받은 객체를 이어지는 블록의 리시버로 전달하며, 블록의 결과값을 반환한다.
```kotlin
    fun <T, R> with(receiver: T, block: T.() -> R): R
```
사실상 `run()`함수와 기능이 거의 동일하다. `run()`함수는 `let()`함수와 `with()`함수를 합쳐놓은 형태라 보아도 무방하다. 즉, `run()`함수를 다음과 같이 표현할 수 있다.
```kotlin
    supportActionBar?.let {
        with(it) {
            setDisplayHomeAsUpEnabled(true)
            setHomeAsUpIndicator(R.drawable.ic_clear_white)    
        }
    }
```
이와 같이 기능은 똑같지만 `run()`함수가 안전한 호출을 지원하는데 반해, `with()`함수는 이를 자체적으로 지원하지 않는다.

출처 : https://www.androidhuman.com/lecture/kotlin/2016/07/06/kotlin_let_apply_run_with/