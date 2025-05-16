# Repository 패턴 기초

## ❓ Repository 패턴이란?

**Repository 패턴**은 ViewModel과 데이터 소스(API, DB 등) 사이에 위치하는 계층입니다.  
데이터를 어디서 가져오든(ViewModel 입장에서는) **일관된 인터페이스로 접근**할 수 있게 해줍니다.

> ViewModel은 Repository에게 "데이터 줘!"라고 요청만 하고, 그 데이터가 네트워크인지, 로컬인지 모른 채 사용할 수 있습니다.

---

## 📌 왜 Repository가 필요한가?

| 문제 상황 | Repository 도입 후 |
|-----------|-------------------|
| ViewModel이 API/DB 코드에 직접 접근 → 책임 과다 | Repository에서 데이터 소스 분리 관리 |
| ViewModel에서 테스트 어려움 | Repository를 Mock 처리하여 테스트 쉬움 |
| 다양한 소스를 함께 사용하려면 로직 복잡 | Repository가 내부에서 적절히 조합 |

---

## 🧭 구조 흐름

```

\[UI] → \[ViewModel] → \[Repository] → \[API / DB / 캐시 등]

````

- ViewModel은 Repository에게만 의존
- Repository는 API, DB, 캐시 등 여러 소스를 결합

---

## ✅ 간단한 예시

### 📄 Repository 인터페이스 정의

```kotlin
interface TodoRepository {
    suspend fun getTodos(): List<Todo>
    suspend fun addTodo(title: String)
}
````

### 📄 실제 구현체

```kotlin
class TodoRepositoryImpl : TodoRepository {
    private val todoList = mutableListOf<Todo>()
    private var nextId = 0

    override suspend fun getTodos(): List<Todo> {
        return todoList
    }

    override suspend fun addTodo(title: String) {
        if (title.isNotBlank()) {
            todoList.add(Todo(id = nextId++, title = title))
        }
    }
}
```

---

## 🧩 ViewModel에서 사용

```kotlin
class TodoViewModel(
    private val repository: TodoRepository
) : ViewModel() {

    private val _todos = MutableStateFlow<List<Todo>>(emptyList())
    val todos: StateFlow<List<Todo>> = _todos

    init {
        viewModelScope.launch {
            _todos.value = repository.getTodos()
        }
    }

    fun addTodo(title: String) {
        viewModelScope.launch {
            repository.addTodo(title)
            _todos.value = repository.getTodos()
        }
    }
}
```

> 여기서는 ViewModel이 데이터 관리 책임을 지지 않고 Repository를 통해 간접 접근합니다.

---

## 💡 확장 아이디어

* `RemoteTodoRepository` → 서버 API 연동
* `LocalTodoRepository` → Room DB 연동
* `TodoRepositoryImpl` → 이 둘을 적절히 조합

---

## 📚 참고 자료

* [Android Developer - Data Layer Architecture](https://developer.android.com/jetpack/guide#recommended-app-arch)
* [Repository Pattern Explained](https://developer.android.com/topic/architecture#repository)
