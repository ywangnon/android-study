# ViewModel과 상태 관리

## 🎯 ViewModel이란?

ViewModel은 **UI에 필요한 데이터를 보관하고 관리**하는 클래스입니다.  
Activity나 Fragment가 화면 회전 등으로 재생성될 때도 데이터를 유지할 수 있게 해줍니다.

> 📌 ViewModel은 View와 Model 사이의 중간다리 역할을 하며, 화면과 관련된 상태(State)를 보존합니다.

---

## 🔄 상태 관리란?

Jetpack Compose는 **상태(State)** 가 바뀌면 자동으로 UI를 다시 그립니다.  
→ ViewModel에서 상태를 관리하고 UI에 전달하면, 뷰는 자동으로 최신 상태를 반영합니다.

---

## 💡 상태 전달 방식: LiveData vs StateFlow

| 항목 | LiveData | StateFlow |
|------|----------|-----------|
| 구현 방식 | ✅ (Compose 외에도 사용 가능) | ✅ (Compose에 최적화됨) |
| Compose 지원 | 가능하지만 따로 옵저버 설정 필요 | `collectAsState()`로 바로 사용 가능 |
| 수동 옵저버 필요 | `observe()` 필요 | 없음 (자동 구독) |
| Null 허용 | 기본 null 허용 | 기본 non-null |

### 결론: **Compose에서는 StateFlow + collectAsState() 조합을 추천**

---

## 🛠️ StateFlow를 사용하는 ViewModel 예시

```kotlin
class CounterViewModel : ViewModel() {
    private val _count = MutableStateFlow(0)
    val count: StateFlow<Int> = _count

    fun increase() {
        _count.value++
    }
}
````

* `_count`: 내부 상태 (변경 가능)
* `count`: 외부에는 읽기 전용으로 공개

---

## 🧱 Compose에서 ViewModel 상태 받기

```kotlin
@Composable
fun CounterScreen(viewModel: CounterViewModel = viewModel()) {
    val count by viewModel.count.collectAsState()

    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = Modifier.fillMaxSize().padding(16.dp)
    ) {
        Text("Count: $count")
        Button(onClick = { viewModel.increase() }) {
            Text("증가")
        }
    }
}
```

### 🔍 설명:

* `collectAsState()`: StateFlow를 Compose에서 자동으로 구독하는 함수
* `by`: Kotlin의 위임 구문 (자동으로 `.value`를 꺼냄)
* UI는 count 값이 바뀔 때마다 자동으로 다시 그려짐

---

## ✅ LiveData도 가능하지만 Compose에는 불편

```kotlin
val count: LiveData<Int> = MutableLiveData(0)

// Compose에서 사용할 경우
val count = viewModel.count.observeAsState()
```

> Compose에는 `StateFlow`가 더 자연스럽고 코드도 간결합니다.

---

## 🧭 ViewModel 사용 흐름 요약

```
[사용자 입력] → View 이벤트
       ↓
[ViewModel] → 상태 변경 함수 호출 (ex. increase())
       ↓
[StateFlow] 값 업데이트
       ↓
[collectAsState()]로 상태 수신
       ↓
UI가 자동으로 최신 상태로 재렌더링
```

---

## 📚 참고 자료

* [State in Jetpack Compose](https://developer.android.com/jetpack/compose/state)
* [ViewModel and StateFlow](https://developer.android.com/topic/libraries/architecture/viewmodel)
