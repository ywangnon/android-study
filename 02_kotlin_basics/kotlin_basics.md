# 2. Kotlin 언어 기초

## ✨ Kotlin이란?

Kotlin은 JetBrains에서 개발하고 Google이 Android 공식 언어로 채택한 프로그래밍 언어입니다.  
간결하고 안전하며 함수형 프로그래밍 요소를 포함하고 있어 Android 개발에 매우 적합합니다.

---

## 🧱 기본 문법

### 변수 선언
```kotlin
val name = "홍길동" // 변경 불가 (Immutable)
var age = 25       // 변경 가능 (Mutable)
````

### 함수 선언

```kotlin
fun greet(name: String): String {
    return "Hello, $name!"
}
```

### 조건문

```kotlin
val score = 80
val grade = if (score >= 90) "A" else "B"
```

### 반복문

```kotlin
for (i in 1..5) {
    println(i)
}

while (age < 30) {
    age++
}
```

---

## 🧭 클래스와 객체

```kotlin
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("저는 $name이고, $age살입니다.")
    }
}

val person = Person("지민", 28)
person.introduce()
```

---

## 🧪 Null Safety

Kotlin은 NullPointerException을 방지하기 위해 null을 안전하게 처리할 수 있는 기능을 제공합니다.

```kotlin
var name: String? = null
println(name?.length)         // null-safe call
println(name?.length ?: 0)    // Elvis operator
```

---

## 🔗 고차 함수와 람다

```kotlin
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {
    return operation(x, y)
}

val sum = calculate(5, 3) { a, b -> a + b }
println(sum) // 8
```

---

## 📚 참고 자료

* [Kotlin 공식 문서](https://kotlinlang.org/docs/home.html)
* [Kotlin Playground](https://play.kotlinlang.org/)
* [Android Kotlin 개발자 가이드](https://developer.android.com/kotlin)
