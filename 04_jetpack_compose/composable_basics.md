# 4. Jetpack Compose 기초

## ✨ Jetpack Compose란?

Jetpack Compose는 Android의 최신 UI 툴킷으로, 기존의 XML 기반 UI 개발 방식 대신 **Kotlin 코드만으로 UI를 구성하는 선언형 방식**을 제공합니다.

### 기존 View 시스템 vs Jetpack Compose

| 항목 | 기존 View 시스템 | Jetpack Compose |
|------|------------------|------------------|
| UI 정의 방식 | XML 레이아웃 파일 | Kotlin 코드 |
| 상태 변경 처리 | findViewById + 수동 갱신 | 상태 기반 자동 재구성 |
| 코드 양 | 많고 복잡 | 간결하고 선언적 |
| 실시간 미리보기 | 제한적 | Composable Preview 지원 |

---

## 🧠 핵심 개념

### 1. `@Composable`
- UI를 구성하는 함수에 붙이는 애노테이션
- 하나의 화면 요소 또는 화면 전체를 정의

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name!")
}
````

---

### 2. `State`와 `remember`

* Compose는 **상태(state)** 에 따라 UI를 자동으로 다시 그립니다.
* `remember`와 `mutableStateOf`를 이용해 상태를 선언하고 유지할 수 있습니다.

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("증가")
        }
    }
}
```

* `remember`: Composable이 다시 호출될 때도 상태를 기억하게 함
* `mutableStateOf`: 값이 바뀌면 UI를 자동으로 갱신함

---

### 3. `Modifier`

* UI 요소의 **크기, 위치, 여백, 배경, 클릭 등 속성**을 설정하는 빌더

```kotlin
Text(
    text = "Hello Compose!",
    modifier = Modifier
        .padding(16.dp)
        .clickable { /* 이벤트 처리 */ }
)
```

> Compose의 `Modifier`는 UIKit의 `UIView` 속성 설정과 유사하지만, 체이닝 방식으로 작성할 수 있습니다.

---

### 4. 기본 레이아웃 Composable

| 이름       | 설명               |
| -------- | ---------------- |
| `Column` | 위에서 아래로 나열       |
| `Row`    | 왼쪽에서 오른쪽으로 나열    |
| `Box`    | 겹치기 또는 정렬 조합에 사용 |
| `Spacer` | 빈 공간 생성에 사용      |

```kotlin
@Composable
fun LayoutExample() {
    Column(modifier = Modifier.padding(16.dp)) {
        Text("이름:")
        TextField(value = "", onValueChange = {})
        Button(onClick = { }) {
            Text("확인")
        }
    }
}
```

---

### 5. `@Preview` - 실시간 미리보기

```kotlin
@Preview(showBackground = true)
@Composable
fun PreviewGreeting() {
    Greeting("Preview")
}
```

* Android Studio에서 **UI를 미리 시각적으로 확인**할 수 있음
* `@Preview`는 실제 앱 실행 없이 결과 확인 가능

---

## 🛠️ Compose 구조의 특징

* 함수 = UI 단위
* `상태 → 자동 UI 반영`
* XML, findViewById가 필요 없음
* 실시간 미리보기와 빠른 빌드 지원

---

## 📌 요약 정리

| 개념       | 키워드                          | 설명                  |
| -------- | ---------------------------- | ------------------- |
| UI 구성 단위 | `@Composable`                | Kotlin 함수로 UI 정의    |
| 상태 관리    | `remember`, `mutableStateOf` | UI 상태를 추적하고 자동 반영   |
| 속성 설정    | `Modifier`                   | 여백, 정렬, 이벤트 등 조절    |
| 구조 배치    | `Column`, `Row`, `Box`       | 레이아웃 배치용 Composable |
| UI 미리보기  | `@Preview`                   | 실시간 코드 미리보기 지원      |

---

## 📚 참고 자료

* [Jetpack Compose 공식 가이드](https://developer.android.com/jetpack/compose)
* [Compose Pathway (Google Developers)](https://developer.android.com/learn/jetpack/compose)
* [Jetpack Compose Samples](https://github.com/android/compose-samples)
