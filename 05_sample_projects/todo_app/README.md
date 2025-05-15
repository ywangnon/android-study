# ✅ Jetpack Compose To-Do 앱 실습

이 실습은 Android 개발을 처음 접하는 분을 위해 만들어졌습니다.  
Jetpack Compose를 사용하여 가장 기본적인 기능들(입력, 목록, 체크박스, 상태 저장 등)을 직접 구현하며 학습합니다.
제가 한 세팅 기준으로 작성했습니다.

---

## 📌 실습 목표

- Jetpack Compose로 화면을 구성하는 방법 이해
- 사용자 입력을 처리하는 방법 익히기
- 상태 기반 UI를 ViewModel로 관리하는 방식 배우기

---

## 🛠️ 1단계: 새 프로젝트 만들기

1. Android Studio 실행
2. **File > New Project > Empty Activity** 선택
3. 프로젝트 이름: `Todo`
4. 최소 SDK: API 33 이상 선택
5. Finish 클릭

> 이 실습은 `Jetpack Compose`가 가능한 최신 템플릿으로 만들어야 합니다.

---

## ⚙️ 2단계: build.gradle 설정

### `build.gradle(:app)` 파일에서 아래 항목을 확인하거나 추가하세요:

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
    // Jetpack Compose 필수
    implementation("androidx.activity:activity-compose:1.8.2")
    implementation("androidx.compose.ui:ui:1.6.1")
    implementation("androidx.compose.material3:material3:1.2.0")
    implementation("androidx.compose.foundation:foundation:1.6.1")

    // 상태 관리용 ViewModel
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.7.0")
}
````

파일 저장 후 화면 상단의 **Sync_Project_with_Gradle_Files** 버튼 클릭!(코끼리 모양)

---

## 🧱 3단계: 파일 구조 만들기

아래와 같이 3개의 폴더와 파일을 생성하세요:

```
com.example.todoapp/
├── MainActivity.kt
├── model/
│   └── Todo.kt
├── viewmodel/
│   └── TodoViewModel.kt
└── ui/
    └── TodoApp.kt
```

---

## 📄 MainActivity.kt – 앱 진입점

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

### 🔍 설명:

* `setContent {}`: Compose에서 UI를 설정할 때 사용
* `viewModel()`: 화면에 상태를 제공하는 TodoViewModel 생성
* `TodoApp()`: 실질적인 화면 UI는 이 함수에서 구성

---

## 📄 model/Todo.kt – 데이터 모델 정의

```kotlin
package com.example.todo.model

data class Todo(
    val id: Int,
    val title: String,
    var isDone: Boolean = false
)
```

### 🔍 설명:

* `Todo`: 하나의 할 일 정보를 저장하는 데이터 클래스
* `id`: 각 할 일의 고유 번호
* `title`: 할 일 내용
* `isDone`: 완료 여부

---

## 📄 viewmodel/TodoViewModel.kt – 상태 관리 담당

```kotlin
package com.example.todo.viewmodel

import androidx.compose.runtime.mutableStateListOf
import androidx.lifecycle.ViewModel
import com.example.todo.model.Todo

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

### 🔍 설명:

* `mutableStateListOf`: Compose가 상태 변화를 감지하고 UI를 자동 업데이트
* `addTodo()`: 새로운 할 일을 리스트에 추가
* `toggleDone()`: 완료 여부를 반전시킴
* `removeDone()`: 체크된 항목들을 모두 삭제

---

## 📄 ui/TodoApp.kt – 화면 UI 구성

```kotlin
package com.example.todo.ui

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

        // 입력창과 버튼
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

        // 할 일 목록 출력
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

        // 완료 항목 삭제 버튼
        Button(onClick = { viewModel.removeDone() }) {
            Text("완료 항목 모두 삭제")
        }
    }
}
```

### 🔍 설명:

* `Column`, `Row`: Compose에서 UI를 배치하는 기본 컨테이너
* `BasicTextField`: 사용자 입력을 받음
* `Checkbox`: 할 일 완료 체크
* `TextDecoration.LineThrough`: 완료된 항목은 줄긋기 표시
* `remember`: 입력 상태를 저장하여 화면 갱신 시 유지

---

## ▶️ 실행 방법

1. USB 연결된 Android 기기 or Emulator 실행
2. Android Studio에서 상단 ▶️ 버튼 클릭
3. 앱이 실행되면:

   * 할 일을 입력하고 ‘추가’ 버튼 클릭
   * 목록에서 항목 클릭 시 체크
   * 하단 버튼으로 체크된 항목 모두 삭제

---

## 📚 마무리 요약

| 학습 내용      | 설명                                        |
| ---------- | ----------------------------------------- |
| Compose 구조 | Composable 함수로 UI 구성                      |
| 상태 관리      | ViewModel + `mutableStateListOf`로 관리      |
| 입력 처리      | `BasicTextField`, `remember` 사용           |
| 화면 구성      | `Column`, `Row`, `Modifier` 활용            |
| 사용자 상호작용   | `clickable`, `Checkbox`, `TextDecoration` |
