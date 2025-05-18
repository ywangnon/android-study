# 9. Android 테스팅과 디버깅 기초

Android 앱 개발에서 테스트와 디버깅은 앱의 **품질을 높이고 문제를 빠르게 해결**하는 데 매우 중요합니다.

---

## 🔍 테스트란?

앱이 의도한 대로 작동하는지 **자동으로 검증**하는 코드입니다.  
Android에서는 다음 두 가지로 나눌 수 있습니다:

| 종류 | 설명 | 예 |
|------|------|----|
| Unit Test | 함수/클래스 단위 로직 테스트 | 계산 함수, Repository 테스트 |
| UI Test | 실제 UI 상호작용 테스트 | 버튼 클릭 → 화면 전환 |

---

## 🧪 Unit Test 기초

### 1. Gradle 설정 (기본 포함되어 있음)

```gradle
testImplementation "junit:junit:4.13.2"
````

### 2. 간단한 테스트 예시

```kotlin
class Calculator {
    fun add(a: Int, b: Int) = a + b
}

class CalculatorTest {

    @Test
    fun testAdd() {
        val calculator = Calculator()
        assertEquals(4, calculator.add(2, 2))
    }
}
```

> 테스트 클래스는 `src/test/java/` 아래 위치하며, `@Test` 어노테이션이 필요합니다.

---

## 🖥️ Compose UI 테스트 기초

### Gradle 설정

```gradle
androidTestImplementation "androidx.compose.ui:ui-test-junit4:1.6.1"
debugImplementation "androidx.compose.ui:ui-test-manifest:1.6.1"
```

### 간단한 Compose 테스트

```kotlin
@get:Rule
val composeTestRule = createComposeRule()

@Test
fun testButtonClick() {
    composeTestRule.setContent {
        var clicked by remember { mutableStateOf(false) }
        Button(onClick = { clicked = true }) {
            Text("Click me")
        }
    }

    composeTestRule
        .onNodeWithText("Click me")
        .performClick()

    // 실제 앱에서는 클릭 후 UI 상태가 변했는지 테스트
}
```

> UI 테스트는 `src/androidTest/` 아래에 작성해야 합니다.

---

## 🧭 Logcat 사용법

Logcat은 Android Studio에서 앱 실행 중의 로그 메시지를 실시간으로 볼 수 있는 도구입니다.

### 기본 로그 출력

```kotlin
Log.d("MyTag", "디버깅용 메시지입니다")
Log.e("MyTag", "에러 발생", exception)
```

| 함수      | 로그 수준           |
| ------- | --------------- |
| `Log.v` | verbose (모든 로그) |
| `Log.d` | debug (개발 중 정보) |
| `Log.i` | info (일반 정보)    |
| `Log.w` | warning (주의 사항) |
| `Log.e` | error (에러 발생)   |

### 실시간 확인

* Android Studio 하단 > **Logcat 탭**
* `TAG` 또는 `PID`로 필터링 가능

---

## 🔧 앱 디버깅 팁

| 도구         | 설명                   |
| ---------- | -------------------- |
| Breakpoint | 특정 코드 라인에서 일시 중단     |
| Debugger   | 변수 값 추적, 흐름 분석       |
| ADB        | 명령어 기반 디바이스 제어 도구    |
| Profiler   | 메모리/CPU 네트워크 사용량 시각화 |

---

## ✅ 디버깅할 때 확인할 것

* **Crash 로그 (Logcat 확인)**
* **NullPointerException 등 런타임 오류**
* **실행 흐름 추적 → breakpoints**
* **API 호출 결과 확인 → 로그 출력**
* **Compose 화면이 안 뜰 때 → Preview, recomposition 원인 추적**

---

## 📚 참고 자료

* [Android Testing Guide](https://developer.android.com/training/testing)
* [Compose Testing](https://developer.android.com/jetpack/compose/testing)
* [Logcat 가이드](https://developer.android.com/studio/debug/logcat)
