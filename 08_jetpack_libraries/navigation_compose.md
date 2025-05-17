# Jetpack Compose에서 Navigation 사용하기

## 🚦 Navigation Component란?

Jetpack의 Navigation Component는 **화면 간 전환을 구조적으로 처리할 수 있도록 도와주는 라이브러리**입니다.  
Compose에서는 `androidx.navigation:navigation-compose` 라이브러리를 사용합니다.

---

## 🎯 Navigation을 쓰는 이유

- 코드 기반으로 화면 전환 처리 가능 (XML 필요 없음)
- 인자 전달이 간편함
- 백스택이 자동 관리됨
- `BottomNavigation`, `Drawer`와도 쉽게 통합 가능

---

## ⚙️ 의존성 추가

### `build.gradle(:app)`에 추가

```gradle
implementation "androidx.navigation:navigation-compose:2.7.7"
````

---

## 🧱 기본 사용 흐름

1. NavHost로 전체 라우팅 구성
2. 각 Composable 화면에 Route 지정
3. NavController로 이동 제어

---

## 📘 실습 예제: 두 화면 이동

> 목표: 첫 화면에서 버튼을 누르면 두 번째 화면으로 이동

---

### 1. `Screen.kt` – 라우트 정의용 enum/const

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Detail : Screen("detail")
}
```

---

### 2. `NavGraph.kt` – 네비게이션 그래프 정의

```kotlin
@Composable
fun AppNavGraph(navController: NavHostController) {
    NavHost(navController, startDestination = Screen.Home.route) {
        composable(Screen.Home.route) {
            HomeScreen(onNavigate = {
                navController.navigate(Screen.Detail.route)
            })
        }
        composable(Screen.Detail.route) {
            DetailScreen()
        }
    }
}
```

---

### 3. `HomeScreen.kt`

```kotlin
@Composable
fun HomeScreen(onNavigate: () -> Unit) {
    Column(modifier = Modifier.fillMaxSize(), horizontalAlignment = Alignment.CenterHorizontally) {
        Text("🏠 Home Screen")
        Button(onClick = onNavigate) {
            Text("Go to Detail")
        }
    }
}
```

---

### 4. `DetailScreen.kt`

```kotlin
@Composable
fun DetailScreen() {
    Column(modifier = Modifier.fillMaxSize(), horizontalAlignment = Alignment.CenterHorizontally) {
        Text("📄 Detail Screen")
    }
}
```

---

### 5. `MainActivity.kt`

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val navController = rememberNavController()
            AppNavGraph(navController = navController)
        }
    }
}
```

---

## 💡 인자 전달 예시

```kotlin
composable("detail/{id}") { backStackEntry ->
    val id = backStackEntry.arguments?.getString("id")
    DetailScreen(id = id)
}

navController.navigate("detail/5")
```

> Route에 파라미터를 포함해 전달하고, backStackEntry를 통해 꺼냅니다.

---

## ✅ 요약

| 구성 요소                      | 역할        |
| -------------------------- | --------- |
| `NavHost`                  | 전체 라우팅 정의 |
| `NavController`            | 이동 제어     |
| `composable()`             | 각 화면 정의   |
| `navigate()`               | 화면 이동     |
| `backStackEntry.arguments` | 인자 꺼내기    |

---

## 📚 참고 링크

* [Compose Navigation 공식 문서](https://developer.android.com/jetpack/compose/navigation)
* [Navigation Compose 샘플](https://developer.android.com/codelabs/jetpack-compose-navigation)
