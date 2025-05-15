# 6. 앱 구조화와 MVVM

## 🧩 왜 앱 구조화가 필요할까?

앱이 복잡해질수록 코드가 뒤엉기기 쉽습니다.  
**View와 로직을 분리하지 않으면 테스트, 유지보수, 협업이 어려워집니다.**

→ 그래서 등장한 구조화 방식이 바로 **MVVM (Model-View-ViewModel)** 입니다.

---

## 🔍 MVVM이란?

| 구성 요소 | 역할 |
|-----------|------|
| Model     | 앱의 비즈니스 로직과 데이터 (예: Repository, API, DB) |
| View      | 화면 UI 요소 (예: Composable 함수, Activity, Fragment) |
| ViewModel | View와 Model 사이에서 상태(state)와 로직을 연결 |

### 예시 흐름:
```

사용자 클릭
→ View 이벤트 발생
→ ViewModel이 처리
→ Model에서 데이터 가져옴
→ ViewModel이 상태 업데이트
→ View가 상태 반영하여 다시 그림

```

---

## ⚙️ 왜 MVVM이 Compose에서 잘 맞을까?

Jetpack Compose는 **선언형 UI**입니다. 즉,  
"이런 상태라면 UI를 이렇게 그려줘!"라는 방식입니다.  
→ `ViewModel`이 상태를 제공하고, `Composable`은 그 상태에 따라 자동으로 UI를 구성하므로 자연스럽게 MVVM 구조와 어울립니다.

---

## 🎯 MVVM의 장점

- View 코드가 단순해진다
- UI 상태와 로직이 명확히 분리된다
- 테스트가 쉬워진다
- 재사용성과 유지보수성이 향상된다

---

## ✏️ 기본 예시 (구성도)

```

\[User 입력]
↓
\[View] → 이벤트 전송
↓
\[ViewModel] → 상태 관리 / 로직 처리
↓
\[Model (Repository)] → 데이터 조회 (API, DB)
↓
\[ViewModel] → 상태 업데이트
↓
\[View] → 자동 리렌더링

```

---

## ✅ 예시 프로젝트 구성 예 (실제 디렉토리 기준)

```

com.example.todoapp/
├── model/
│   └── Todo.kt
├── viewmodel/
│   └── TodoViewModel.kt
├── repository/
│   └── TodoRepository.kt
├── di/
│   └── AppModule.kt  ← Hilt로 주입 설정
├── ui/
│   └── TodoApp.kt
└── MainActivity.kt

```

---

## 📚 참고 자료

- [Android 공식 MVVM 가이드](https://developer.android.com/topic/architecture)
- [Compose와 상태 관리](https://developer.android.com/jetpack/compose/state)
