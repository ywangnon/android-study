# Compose UI 테스트 & ViewModel 테스트 실습

## 🧪 1. Compose UI 테스트란?

Compose UI 테스트는 실제 사용자의 행동처럼 **버튼 클릭, 텍스트 입력, UI 상태 확인** 등을 자동으로 검증합니다.

> 화면이 정상 작동하는지 수동으로 확인하지 않아도, 코드로 테스트할 수 있습니다.

---

## ⚙️ 기본 Gradle 설정

```gradle
androidTestImplementation "androidx.compose.ui:ui-test-junit4:1.6.1"
debugImplementation "androidx.compose.ui:ui-test-manifest:1.6.1"
````

---

## 🧱 2. Compose UI 테스트 예제

```kotlin
@get:Rule
val composeTestRule = createComposeRule()

@Test
fun testButtonClickChangesText() {
    composeTestRule.setContent {
        var message by remember { mutableStateOf("Hello") }

        Column {
            Text(text = message)
            Button(onClick = { message = "Clicked!" }) {
                Text("Click me")
            }
        }
    }

    // 텍스트가 "Hello"인지 확인
    composeTestRule
        .onNodeWithText("Hello")
        .assertExists()

    // 버튼 클릭
    composeTestRule
        .onNodeWithText("Click me")
        .performClick()

    // 텍스트가 "Clicked!"로 바뀌었는지 확인
    composeTestRule
        .onNodeWithText("Clicked!")
        .assertExists()
}
```

### 🔍 설명:

* `onNodeWithText()`로 요소를 찾고
* `performClick()`으로 클릭 실행
* `assertExists()`로 결과 확인

---

## 🧪 3. ViewModel 테스트란?

ViewModel 테스트는 UI 없이도 **상태 변경과 로직이 올바른지 확인**할 수 있는 단위 테스트입니다.

---

## ⚙️ 테스트 설정 (JUnit + Coroutine Test)

```gradle
testImplementation "junit:junit:4.13.2"
testImplementation "org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.3"
```

---

## 🧠 ViewModel 예제

```kotlin
class CounterViewModel : ViewModel() {
    private val _count = MutableStateFlow(0)
    val count: StateFlow<Int> = _count

    fun increase() {
        _count.value++
    }
}
```

---

## 🧪 ViewModel 테스트 코드

```kotlin
@ExperimentalCoroutinesApi
class CounterViewModelTest {

    private val testDispatcher = StandardTestDispatcher()

    @Before
    fun setup() {
        Dispatchers.setMain(testDispatcher)
    }

    @Test
    fun testIncrease() = runTest {
        val viewModel = CounterViewModel()
        viewModel.increase()
        assertEquals(1, viewModel.count.value)
    }

    @After
    fun tearDown() {
        Dispatchers.resetMain()
    }
}
```

### 🔍 설명:

* `runTest`: 코루틴 테스트 전용 함수
* `StandardTestDispatcher`: 테스트를 위한 Dispatcher
* `assertEquals`: 예상 값과 실제 값 비교

---

## ✅ 요약 정리

| 테스트 종류 | 대상            | 위치                 | 프레임워크                  |
| ------ | ------------- | ------------------ | ---------------------- |
| UI 테스트 | Composable 화면 | `src/androidTest/` | Compose UI Test        |
| 단위 테스트 | ViewModel, 함수 | `src/test/`        | JUnit + Coroutine Test |

---

## 📚 참고 링크

* [Compose UI 테스트 문서](https://developer.android.com/jetpack/compose/testing)
* [ViewModel 테스트 공식 가이드](https://developer.android.com/jetpack/guide#testing)
