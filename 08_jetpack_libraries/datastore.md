# DataStore 기본 사용법 (Jetpack Key-Value 저장소)

## 💾 DataStore란?

DataStore는 Google이 SharedPreferences의 한계를 극복하기 위해 만든 **새로운 키-값 기반 저장소**입니다.  
**비동기 + 코루틴 기반 + 타입 안정성**을 특징으로 하며, 설정값 저장이나 간단한 상태 유지에 적합합니다.

---

## 🔍 SharedPreferences vs DataStore

| 항목 | SharedPreferences | DataStore |
|------|-------------------|-----------|
| 동기/비동기 | 동기 (메인 스레드 위험) | 비동기 (코루틴 기반) |
| 방식 | XML 기반 | Kotlin Flow 기반 |
| 타입 안전성 | 약함 | 강함 |
| 구조화 | 어려움 | Proto 버전으로 가능 |
| Compose 연동 | 수동 필요 | Flow로 자연스럽게 연동 가능 |

---

## 🧩 DataStore 종류

| 종류 | 설명 |
|------|------|
| Preferences DataStore | 키-값 기반 (기존 SharedPreferences 대체) |
| Proto DataStore | Schema 기반 구조화 저장 (ProtoBuf 사용) |

> 여기서는 **Preferences DataStore**만 다룹니다.

---

## 📦 Gradle 의존성 추가

```gradle
implementation "androidx.datastore:datastore-preferences:1.0.0"
````

---

## 🛠️ 기본 설정 (싱글톤 방식)

```kotlin
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")
```

> 이 코드는 `Context`의 확장 프로퍼티로 사용합니다 (예: Application, Activity)

---

## 📌 키 정의

```kotlin
object SettingsKeys {
    val DARK_MODE = booleanPreferencesKey("dark_mode")
}
```

---

## 🧠 저장하기

```kotlin
suspend fun saveDarkMode(context: Context, isDarkMode: Boolean) {
    context.dataStore.edit { preferences ->
        preferences[SettingsKeys.DARK_MODE] = isDarkMode
    }
}
```

---

## 📖 불러오기

```kotlin
fun readDarkMode(context: Context): Flow<Boolean> {
    return context.dataStore.data
        .map { preferences ->
            preferences[SettingsKeys.DARK_MODE] ?: false
        }
}
```

---

## 🖥️ Compose에서 사용 예시

```kotlin
@Composable
fun DarkModeSettingScreen(context: Context) {
    val darkModeFlow = remember { readDarkMode(context) }
    val isDarkMode by darkModeFlow.collectAsState(initial = false)

    Column(modifier = Modifier.padding(16.dp)) {
        Text("다크 모드 설정: ${if (isDarkMode) "ON" else "OFF"}")
        Button(onClick = {
            CoroutineScope(Dispatchers.IO).launch {
                saveDarkMode(context, !isDarkMode)
            }
        }) {
            Text("토글")
        }
    }
}
```

---

## 💡 실제 앱에서의 활용 예시

* 로그인 유지 상태 저장 (`isLoggedIn`)
* 다크모드 설정 (`isDarkMode`)
* 자동로그인 / 언어설정 등 앱 설정 저장

---

## ✅ DataStore 요약

```
[preferencesDataStore] → Context 확장
[Preferences Key] → 저장 키 정의
edit() → 저장
data.map {} → 읽기
collectAsState() → Compose 상태 연동
```

---

## 📚 참고 자료

* [DataStore 공식 문서](https://developer.android.com/topic/libraries/architecture/datastore)
* [Preferences DataStore 코드랩](https://developer.android.com/codelabs/jetpack-datastore)
