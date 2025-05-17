# Retrofit API 연동 실습 예제

이 실습에서는 무료 공개 API를 사용하여 Retrofit을 통해 데이터를 가져오고, ViewModel과 Compose로 화면에 출력하는 전체 흐름을 구성해봅니다.

---

## 🎯 실습 목표

- Retrofit 인터페이스 정의
- ViewModel에서 API 호출
- 상태를 StateFlow로 관리
- Compose에서 리스트로 표시

---

## 🌐 사용할 API

- URL: https://jsonplaceholder.typicode.com/todos
- 설명: 샘플 할 일 목록(Todo) JSON 리스트 제공

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
  },
  ...
]
````

---

## 🧱 1단계: 데이터 모델 정의

```kotlin
data class TodoResponse(
    val id: Int,
    val title: String,
    val completed: Boolean
)
```

> 서버에서 내려오는 JSON 구조에 맞게 변수명을 동일하게 맞추는 것이 중요합니다.

---

## 🔌 2단계: Retrofit 인터페이스 정의

```kotlin
interface TodoApi {
    @GET("todos")
    suspend fun getTodos(): List<TodoResponse>
}
```

---

## ⚙️ 3단계: Retrofit 객체 생성

```kotlin
object RetrofitClient {
    private const val BASE_URL = "https://jsonplaceholder.typicode.com/"

    val api: TodoApi by lazy {
        Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
            .create(TodoApi::class.java)
    }
}
```

---

## 🧠 4단계: ViewModel에서 API 호출

```kotlin
class TodoApiViewModel : ViewModel() {

    private val _todos = MutableStateFlow<List<TodoResponse>>(emptyList())
    val todos: StateFlow<List<TodoResponse>> = _todos

    init {
        viewModelScope.launch {
            try {
                _todos.value = RetrofitClient.api.getTodos()
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }
}
```

---

## 🖥️ 5단계: Compose에서 출력

```kotlin
@Composable
fun TodoApiScreen(viewModel: TodoApiViewModel = viewModel()) {
    val todoList by viewModel.todos.collectAsState()

    LazyColumn(modifier = Modifier.padding(16.dp)) {
        items(todoList) { todo ->
            Row(modifier = Modifier
                .fillMaxWidth()
                .padding(vertical = 8.dp)) {
                Checkbox(
                    checked = todo.completed,
                    onCheckedChange = null,
                    enabled = false
                )
                Text(text = todo.title, modifier = Modifier.padding(start = 8.dp))
            }
        }
    }
}
```

---

## 🔧 6단계: MainActivity 연결

```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TodoApiScreen()
            }
        }
    }
}
```

> Hilt를 사용하지 않는다면 `@AndroidEntryPoint` 제거하고 `viewModel()`로 직접 불러도 됩니다.

---

## 📦 build.gradle 설정

```gradle
dependencies {
    implementation "com.squareup.retrofit2:retrofit:2.9.0"
    implementation "com.squareup.retrofit2:converter-gson:2.9.0"

    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.7.0"
    implementation "androidx.lifecycle:lifecycle-runtime-ktx:2.7.0"

    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
}
```

---

## ✅ 요약 흐름

```
[RetrofitClient] → TodoApi.getTodos()
       ↓
[ViewModel] → 결과를 MutableStateFlow에 저장
       ↓
[Composable] → collectAsState()로 UI 갱신
```

---

## 📚 참고

* [JSONPlaceholder 공식](https://jsonplaceholder.typicode.com/)
* [Retrofit 공식 문서](https://square.github.io/retrofit/)
* [Jetpack Compose + Retrofit 예제](https://developer.android.com/jetpack/compose/architecture)
