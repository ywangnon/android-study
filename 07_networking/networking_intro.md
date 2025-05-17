# 7. 네트워크 & API 연동

## 🌐 API란?

API(Application Programming Interface)는 **앱이 외부 서버와 데이터를 주고받기 위한 통신 규칙**입니다.  
Android 앱은 보통 서버에서 데이터를 가져오기 위해 **HTTP 요청을 보내고 JSON 응답을 받습니다.**

예:
- 날씨 앱 → 기상청 API
- 뉴스 앱 → 뉴스 제공사 API
- 쇼핑 앱 → 상품/결제 API

---

## 🚀 Retrofit이란?

`Retrofit`은 Android에서 가장 널리 사용되는 **HTTP 클라이언트 라이브러리**입니다.  
복잡한 네트워크 통신 코드를 간단한 인터페이스로 처리할 수 있게 도와줍니다.

### 주요 특징
- REST API 호출을 쉽게 정의
- Gson, Moshi 등 JSON 변환기와 통합 가능
- 코루틴, RxJava 등과 함께 사용 가능

---

## ⚙️ Retrofit 기본 구성

```kotlin
interface ApiService {
    @GET("todos")
    suspend fun getTodos(): List<Todo>
}
````

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://jsonplaceholder.typicode.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()

val api = retrofit.create(ApiService::class.java)
```

---

## 🧩 Retrofit 구성요소 설명

| 구성요소                               | 역할                                |
| ---------------------------------- | --------------------------------- |
| `@GET`, `@POST`, `@PUT`, `@DELETE` | 어떤 HTTP 요청을 보낼지 정의                |
| `suspend`                          | 코루틴 기반 비동기 호출 표시                  |
| `baseUrl()`                        | 기본 URL 주소                         |
| `ConverterFactory`                 | JSON을 객체로 변환하는 역할 (Gson, Moshi 등) |

---

## 📦 프로젝트 설정 방법

### `build.gradle(:app)`에 추가

```gradle
// Retrofit & Gson
implementation "com.squareup.retrofit2:retrofit:2.9.0"
implementation "com.squareup.retrofit2:converter-gson:2.9.0"

// Coroutine
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3"
```

---

## 🧭 간단한 흐름

```
[ViewModel] → API 호출
   ↓
[Retrofit] → 서버에 HTTP 요청
   ↓
[서버 응답 (JSON)] → Gson으로 파싱
   ↓
[ViewModel] → 상태 업데이트
   ↓
[Compose] → 자동 UI 갱신
```

---

## 💡 참고 개념: REST API란?

REST API는 **HTTP 요청(GET, POST 등)을 기반으로 하는 데이터 통신 방식**입니다.

| 요청 종류  | 설명     |
| ------ | ------ |
| GET    | 데이터 조회 |
| POST   | 데이터 추가 |
| PUT    | 데이터 수정 |
| DELETE | 데이터 삭제 |

---

## 📚 참고 자료

* [Retrofit 공식 문서](https://square.github.io/retrofit/)
* [JSONPlaceholder - 무료 테스트 API](https://jsonplaceholder.typicode.com/)
* [Android Networking Guide](https://developer.android.com/training/volley)
