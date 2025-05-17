# Room 데이터베이스 기본 사용법 (Compose 기반)

## 🗃️ Room이란?

Room은 SQLite의 상위 추상화 라이브러리로, SQL을 직접 작성하지 않아도 **안정적이고 타입 안전한 데이터베이스 작업**을 할 수 있게 해줍니다.

---

## 🧩 Room의 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| `@Entity` | 데이터베이스 테이블을 나타내는 데이터 클래스 |
| `@Dao` | 데이터 접근 객체 (SQL 실행 인터페이스) |
| `@Database` | Room 데이터베이스 설정 클래스 |

---

## 📦 1. Gradle 의존성 추가

```gradle
implementation "androidx.room:room-runtime:2.6.1"
kapt "androidx.room:room-compiler:2.6.1"
implementation "androidx.room:room-ktx:2.6.1"
````

> `kapt`을 쓰기 위해 `apply plugin: 'kotlin-kapt'` 또는 `plugins { id 'kotlin-kapt' }` 도 설정해야 합니다.

---

## 🧱 2. Entity 정의

```kotlin
@Entity(tableName = "todo_table")
data class TodoEntity(
    @PrimaryKey(autoGenerate = true) val id: Int = 0,
    val title: String,
    val isDone: Boolean = false
)
```

> 각 프로퍼티는 컬럼이 되며, `@PrimaryKey`로 기본키를 지정합니다.

---

## 🧠 3. DAO 정의

```kotlin
@Dao
interface TodoDao {
    @Query("SELECT * FROM todo_table")
    fun getAllTodos(): Flow<List<TodoEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(todo: TodoEntity)

    @Delete
    suspend fun delete(todo: TodoEntity)
}
```

> `Flow<List<TodoEntity>>`를 반환하면 Compose에서 자동으로 상태 추적이 가능합니다.

---

## 🏛️ 4. Database 정의

```kotlin
@Database(entities = [TodoEntity::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun todoDao(): TodoDao
}
```

---

## ⚙️ 5. Database 인스턴스 만들기

```kotlin
object DatabaseProvider {
    fun getDatabase(context: Context): AppDatabase {
        return Room.databaseBuilder(
            context,
            AppDatabase::class.java,
            "todo_database"
        ).build()
    }
}
```

---

## 🧠 6. ViewModel에서 Room 사용

```kotlin
class TodoRoomViewModel(
    private val dao: TodoDao
) : ViewModel() {

    val todos: StateFlow<List<TodoEntity>> = dao.getAllTodos()
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(), emptyList())

    fun addTodo(title: String) {
        viewModelScope.launch {
            dao.insert(TodoEntity(title = title))
        }
    }

    fun deleteTodo(todo: TodoEntity) {
        viewModelScope.launch {
            dao.delete(todo)
        }
    }
}
```

---

## 🖥️ 7. Composable에서 출력

```kotlin
@Composable
fun TodoRoomScreen(viewModel: TodoRoomViewModel) {
    val todos by viewModel.todos.collectAsState()

    LazyColumn {
        items(todos) { todo ->
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(8.dp)
            ) {
                Text(todo.title, modifier = Modifier.weight(1f))
                Button(onClick = { viewModel.deleteTodo(todo) }) {
                    Text("삭제")
                }
            }
        }
    }
}
```

---

## 💡 ViewModel에 DAO를 주입하려면?

* Hilt를 사용해 `@Provides fun provideTodoDao()`로 주입
* 또는 수동으로 ViewModelFactory에서 DAO를 전달

---

## ✅ Room 요약 흐름

```
[Entity] → 테이블 정의
[DAO] → 데이터 접근 인터페이스
[Database] → Room 인스턴스 생성
[ViewModel] → suspend 함수로 DB 호출
[Composable] → 상태를 collect하여 UI에 반영
```

---

## 📚 참고 링크

* [Room 공식 문서](https://developer.android.com/jetpack/androidx/releases/room)
* [Room with Compose 가이드](https://developer.android.com/jetpack/compose/libraries#room)
