# DI(Dependency Injection)와 Hilt 소개

## ❓ DI란 무엇인가?

DI(Dependency Injection)는 **필요한 객체를 직접 생성하지 않고 외부에서 주입받는 설계 패턴**입니다.

### 직접 생성 vs 주입

```kotlin
// 직접 생성
val repository = TodoRepositoryImpl()

// 주입 받기
class TodoViewModel(val repository: TodoRepository)
````

> DI를 사용하면 **코드가 느슨하게 결합(loose coupling)** 되어 **유지보수, 테스트, 확장**이 쉬워집니다.

---

## 🤔 왜 Android에서 DI가 중요할까?

| 문제점                            | DI로 해결되는 방식                   |
| ------------------------------ | ----------------------------- |
| ViewModel에서 직접 객체 생성 → 재사용 어려움 | 외부에서 생성된 객체를 주입               |
| 테스트 시 Mock 주입이 어려움             | 주입만 바꾸면 테스트용 구현도 가능           |
| App 전체에 같은 인스턴스를 써야 하는 경우 어려움  | 싱글톤 제공 가능 (예: Retrofit, DB 등) |

---

## ✅ Android DI 대표 도구

* **Hilt**: Google이 제공하는 Android 전용 DI 도구. Dagger 기반.
* Dagger: Hilt의 기반 라이브러리
* Koin: Kotlin 친화적인 DI 프레임워크 (대체제로 존재)

---

## ⚙️ Hilt 설정 방법

### 📦 1. build.gradle 설정

```gradle
// build.gradle(:project)
buildscript {
    dependencies {
        classpath "com.google.dagger:hilt-android-gradle-plugin:2.48"
    }
}
```

```gradle
// build.gradle(:app)
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
    id 'kotlin-kapt'
    id 'dagger.hilt.android.plugin'
}

dependencies {
    implementation "com.google.dagger:hilt-android:2.48"
    kapt "com.google.dagger:hilt-compiler:2.48"
}
```

### 🔄 이후 Sync Now 클릭

---

## 📂 2. Application 클래스 생성

```kotlin
@HiltAndroidApp
class MyApplication : Application()
```

> 이 클래스를 `AndroidManifest.xml`의 `application` 항목에 등록:

```xml
<application
    android:name=".MyApplication"
    ...>
```

---

## 🧱 3. 의존성 주입 모듈 만들기

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    @Singleton
    fun provideTodoRepository(): TodoRepository {
        return TodoRepositoryImpl()
    }
}
```

---

## 🧩 4. ViewModel에서 주입받기

```kotlin
@HiltViewModel
class TodoViewModel @Inject constructor(
    private val repository: TodoRepository
) : ViewModel() {
    ...
}
```

> `@Inject`가 붙은 생성자를 통해 Hilt가 자동으로 주입합니다.

---

## 📄 5. Activity 또는 Composable에서 ViewModel 사용

```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            val viewModel: TodoViewModel = hiltViewModel()
            TodoApp(viewModel = viewModel)
        }
    }
}
```

> `@AndroidEntryPoint`를 Activity에 붙여야 Hilt가 작동합니다.

---

## 🧭 전체 흐름 요약

```
[AppModule] → 의존성 정의
   ↓
[Hilt] → ViewModel 생성 시 의존성 자동 주입
   ↓
[ViewModel] → 주입된 객체 사용
   ↓
[View or Compose] → ViewModel 연결
```

---

## ✅ Hilt의 장점 요약

* 의존성 주입을 쉽게 자동화
* 싱글톤 관리 편리
* Android 구성요소들과 통합 잘 됨
* ViewModel/Activity/Fragment 등에서 바로 사용 가능

---

## 📚 참고 링크

* [Hilt 공식 문서](https://developer.android.com/training/dependency-injection/hilt-android)
* [Hilt with ViewModel & Compose](https://developer.android.com/jetpack/compose/architecture#hilt)
