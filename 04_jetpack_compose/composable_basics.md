# 4. Jetpack Compose 기초

## ✨ Jetpack Compose란?

Jetpack Compose는 Android의 **현대적인 UI 툴킷**으로, 기존의 XML 기반 UI 구성 방식 대신 **Kotlin 코드로 UI를 선언형으로 작성**할 수 있도록 해줍니다.

---

## 🧱 기본 개념

- `@Composable`: Compose에서 UI를 정의할 때 사용하는 함수
- `State`: UI 상태를 추적하고 자동으로 UI를 업데이트
- `Modifier`: 레이아웃, 스타일, 동작 등을 조절하는 도구

---

## ✅ Composable 함수 예시

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name!")
}
````

```kotlin
@Composable
fun MyScreen() {
    Column {
        Greeting("지민")
        Greeting("철수")
    }
}
```

---

## 🧭 상태 관리 - remember & mutableStateOf

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text(text = "카운트: $count")
        Button(onClick = { count++ }) {
            Text("증가")
        }
    }
}
```

* `remember`: Composable 함수가 다시 호출되어도 상태를 유지
* `mutableStateOf`: 상태를 보유하고 변경될 때 자동으로 UI 재구성

---

## 🎨 기본 레이아웃

Compose는 전통적인 `ViewGroup` 대신 **Column, Row, Box** 등의 Composable로 UI를 구성합니다.

```kotlin
@Composable
fun LayoutExample() {
    Column(
        modifier = Modifier.padding(16.dp)
    ) {
        Text("이름을 입력하세요:")
        TextField(value = "", onValueChange = {})
        Button(onClick = { }) {
            Text("확인")
        }
    }
}
```

---

## 🔗 Modifier 예시

```kotlin
Text(
    text = "Compose!",
    modifier = Modifier
        .padding(8.dp)
        .background(Color.Yellow)
        .clickable { /* 클릭 이벤트 */ }
)
```

---

## 🧪 Preview 기능

```kotlin
@Preview(showBackground = true)
@Composable
fun PreviewGreeting() {
    Greeting("Preview")
}
```

* Android Studio에서 실시간으로 Composable UI 미리보기 가능

---

## 📚 참고 자료

* [Jetpack Compose 공식 가이드](https://developer.android.com/jetpack/compose)
* [Compose Pathway (Google Developers)](https://developer.android.com/learn/jetpack/compose)
* [Jetpack Compose Samples](https://github.com/android/compose-samples)
