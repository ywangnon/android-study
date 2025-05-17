# 비동기 처리와 코루틴 (Coroutines)

## ⏳ 비동기 처리란?

비동기 처리란 시간이 오래 걸리는 작업(API 호출, 파일 읽기 등)을 **메인 스레드와 분리해서 처리**하는 방법입니다.  
안드로이드에서 네트워크 작업은 반드시 비동기로 실행해야 하며, 그렇지 않으면 앱이 멈추고 강제 종료됩니다.

---

## 🌀 코루틴(Coroutine)이란?

Kotlin의 공식 비동기 처리 방법입니다.  
기존의 `Thread`, `AsyncTask`, `Callback` 방식보다 더 **간결하고 안전하며 효율적**입니다.

---

## 🚀 Coroutine의 핵심 개념

| 용어 | 설명 |
|------|------|
| `suspend` 함수 | 코루틴 안에서만 실행 가능한 함수 (일시 중단 가능) |
| `launch` | 새 코루틴을 시작하는 함수 (결과 X) |
| `async/await` | 결과가 필요한 경우 (ex. API 응답) |
| `viewModelScope` | ViewModel에서 사용하는 CoroutineScope |
| `Dispatchers.IO` | I/O 작업에 최적화된 스레드 사용 |

---

## ✅ Retrofit과 함께 코루틴 사용하기

Retrofit의 API는 `suspend` 키워드를 붙이면 자동으로 코루틴 기반이 됩니다.

### 예시:

```kotlin
interface TodoApi {
    @GET("todos")
    suspend fun getTodos(): List<TodoResponse>
}
````

> 위 코드는 비동기로 동작하며, ViewModel이나 CoroutineScope에서 호출할 수 있습니다.

---

## 🧠 ViewModel에서 코루틴 사용 예시

```kotlin
class TodoApiViewModel : ViewModel() {

    private val _todos = MutableStateFlow<List<TodoResponse>>(emptyList())
    val todos: StateFlow<List<TodoResponse>> = _todos

    init {
        viewModelScope.launch {
            try {
                val result = RetrofitClient.api.getTodos()
                _todos.value = result
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }
}
```

### 🔍 설명:

* `viewModelScope`: ViewModel이 살아 있는 동안 유지되는 CoroutineScope
* `launch`: 코루틴을 시작 (비동기 실행)
* `try-catch`: 네트워크 실패 시 앱이 죽지 않도록 예외 처리

---

## 🛠️ Coroutine 설정 방법

### 📦 build.gradle(\:app)

```gradle
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
```

---

## 🧪 Compose 연동 흐름 정리

```
[ViewModel]
  → viewModelScope.launch { api 호출 }
     → suspend 함수로 네트워크 요청
        → 응답을 StateFlow에 저장
           → Compose에서 collectAsState()로 구독
              → UI 자동 갱신
```

---

## 💡 코루틴을 사용할 때 주의점

* **UI 업데이트는 반드시 Main 스레드에서** 해야 함 → `Dispatchers.Main`
* 네트워크 호출은 **I/O 스레드에서 처리** → `Dispatchers.IO`
* `viewModelScope` 안에서는 Dispatcher 설정 없이도 기본 설정이 적절히 적용됨

---

## 📚 참고 자료

* [Kotlin Coroutine 공식 가이드](https://kotlinlang.org/docs/coroutines-overview.html)
* [Android Coroutine 가이드](https://developer.android.com/kotlin/coroutines)
* [Coroutine + Retrofit 실습 예제](https://developer.android.com/topic/libraries/architecture/coroutines)
