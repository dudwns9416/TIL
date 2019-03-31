
```kotlin
    fun main(){
        var obj = Address()
        println(obj.name) // 결과값 Kotlin!!!
    }

    class Address {
        var name: String = "Kotlin"
        //getter와 setter 재정의
        get() {``````````````````````````````````````````
        
            return field + "!!!"
        }
        set(value) { field = value }
    }
```