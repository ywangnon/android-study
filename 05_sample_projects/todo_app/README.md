# ✅ Jetpack Compose To-Do 앱 실습

이 실습은 Android 초보자를 위한 실습입니다.  
Jetpack Compose와 ViewModel을 사용해 상태 기반 UI를 직접 구현해보며 Android 앱 구조를 익힙니다.

---

## 📌 주요 기능

- 새로운 할 일 추가
- 완료 여부 체크
- 완료된 항목 삭제
- ViewModel로 상태 관리

---

## 🗂️ 파일 구성

```text
todo_app/
├── MainActivity.kt
├── model/
│   └── Todo.kt
├── viewmodel/
│   └── TodoViewModel.kt
└── ui/
    └── TodoApp.kt
````

---

## 🛠️ 1. 새 프로젝트 생성

1. Android Studio 실행
2. **New Project > Empty Activity** 선택
3. 이름은 예: `TodoApp`, 최소 SDK는 **API 33 이상** 권장
4. Finish

---

## ⚙️ 2. build.gradle 설정

`build.gradle(:app)` 파일을 열고 아래 항목을 확인하거나 수정합니다:

```gradle
android {
    buildFeatures {
        compose = true
    }
    composeOptions {
        kotlinCompilerExtensionVersion = "1.5.8"
    }
}

dependencies {
    implementation("androidx.activity:activity-compose:1.8.2")
    implementation("androidx.compose.ui:ui:1.6.1")
    implementation("androidx.compose.material3:material3:1.2.0")
    implementation("androidx.compose.foundation:foundation:1.6.1")

    // ViewModel with Compose
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
}
```

상단 메뉴의 `File > Sync Project with Gradle Files` 실행

---

## 🧱 3. 코드 작성

> 💡 기본 패키지는 `com.example.todoapp` 으로 가정합니다.

### 📄 `MainActivity.kt`

```kotlin
package com.example.todo

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.material3.MaterialTheme
import androidx.lifecycle.viewmodel.compose.viewModel
import com.example.todo.ui.TodoApp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TodoApp(viewModel = viewModel())
            }
        }
    }
}
```

---

### 📄 `model/Todo.kt`

```kotlin
package com.example.todo.model

data class Todo(
    val id: Int,
    val title: String,
    var isDone: Boolean = false
)
```

---

### 📄 `viewmodel/TodoViewModel.kt`

```kotlin
package com.example.todoapp.viewmodel

import androidx.compose.runtime.mutableStateListOf
import androidx.lifecycle.ViewModel
import com.example.todoapp.model.Todo

class TodoViewModel : ViewModel() {
    private var nextId = 0
    var todoList = mutableStateListOf<Todo>()
        private set

    fun addTodo(title: String) {
        if (title.isNotBlank()) {
            todoList.add(Todo(nextId++, title))
        }
    }

    fun toggleDone(todo: Todo) {
        todo.isDone = !todo.isDone
    }

    fun removeDone() {
        todoList.removeAll { it.isDone }
    }
}
```

---

### 📄 `ui/TodoApp.kt`

```kotlin
package com.example.todoapp.ui

import androidx.compose.foundation.border
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.text.BasicTextField
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.style.TextDecoration
import androidx.compose.ui.unit.dp
import androidx.compose.ui.Alignment
import com.example.todo.viewmodel.TodoViewModel
import com.example.todo.model.Todo

@Composable
fun TodoApp(viewModel: TodoViewModel) {
    var input by remember { mutableStateOf("") }

    Column(modifier = Modifier
        .fillMaxSize()
        .padding(16.dp)) {

        Row(verticalAlignment = Alignment.CenterVertically) {
            BasicTextField(
                value = input,
                onValueChange = { input = it },
                textStyle = TextStyle.Default,
                modifier = Modifier
                    .weight(1f)
                    .border(1.dp, MaterialTheme.colorScheme.primary)
                    .padding(8.dp)
            )
            Spacer(modifier = Modifier.width(8.dp))
            Button(onClick = {
                viewModel.addTodo(input)
                input = ""
            }) {
                Text("추가")
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        for (todo in viewModel.todoList) {
            Row(
                verticalAlignment = Alignment.CenterVertically,
                modifier = Modifier
                    .fillMaxWidth()
                    .clickable { viewModel.toggleDone(todo) }
                    .padding(vertical = 8.dp)
            ) {
                Checkbox(
                    checked = todo.isDone,
                    onCheckedChange = { viewModel.toggleDone(todo) }
                )
                Text(
                    text = todo.title,
                    modifier = Modifier.padding(start = 8.dp),
                    textDecoration = if (todo.isDone) TextDecoration.LineThrough else TextDecoration.None
                )
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        Button(onClick = { viewModel.removeDone() }) {
            Text("완료 항목 모두 삭제")
        }
    }
}
```

---

## ▶️ 4. 실행

1. USB 연결된 Android 기기 또는 Emulator 실행
2. Android Studio 상단 ▶️ 버튼 클릭

## 📚 참고 자료

* [Jetpack Compose 공식 문서](https://developer.android.com/jetpack/compose)
* [ViewModel 공식 가이드](https://developer.android.com/topic/libraries/architecture/viewmodel)
