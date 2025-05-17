# Gson과 Moshi로 JSON 파싱하기

Retrofit은 서버에서 받은 JSON 응답을 Kotlin 객체로 자동 변환하기 위해 **Converter(변환기)** 를 필요로 합니다.

대표적인 JSON 파서:
- **Gson**: Google이 만든 오래된 JSON 파서 (Retrofit 기본 지원)
- **Moshi**: Square에서 만든 최신 JSON 파서 (Kotlin 친화적)

---

## 🤖 Gson이란?

- Google이 개발한 JSON 파서
- `@SerializedName`으로 JSON 키와 변수명을 연결
- 널 허용/기본값 등 설정은 제한적
- Kotlin보다 Java에 최적화

### 예시:

```kotlin
data class TodoResponse(
    @SerializedName("id") val id: Int,
    @SerializedName("title") val title: String,
    @SerializedName("completed") val completed: Boolean
)
````

> `@SerializedName`을 쓰지 않으면 JSON 키와 변수명이 완전히 같아야 매핑됩니다.

---

## ⚙️ Gson 사용 설정

### build.gradle:

```gradle
implementation "com.squareup.retrofit2:converter-gson:2.9.0"
```

### Retrofit 등록:

```kotlin
Retrofit.Builder()
    .baseUrl(BASE_URL)
    .addConverterFactory(GsonConverterFactory.create())
```

---

## 🐻 Moshi란?

* Square에서 만든 Kotlin 중심 JSON 파서
* `@Json`으로 키 매핑
* `kotlinx.serialization`보다 더 실용적인 경우 많음
* 더 안전한 null 처리와 Kotlin 지원

### 예시:

```kotlin
@JsonClass(generateAdapter = true)
data class TodoResponse(
    @Json(name = "id") val id: Int,
    @Json(name = "title") val title: String,
    @Json(name = "completed") val completed: Boolean
)
```

> `@JsonClass(generateAdapter = true)`는 Moshi가 어댑터를 자동 생성하기 위해 필요합니다.

---

## ⚙️ Moshi 사용 설정

### build.gradle:

```gradle
implementation "com.squareup.moshi:moshi:1.14.0"
implementation "com.squareup.moshi:moshi-kotlin:1.14.0"
implementation "com.squareup.retrofit2:converter-moshi:2.9.0"
kapt "com.squareup.moshi:moshi-kotlin-codegen:1.14.0"
```

> Moshi는 코드 생성이 필요하므로 `kapt`와 `apply plugin: 'kotlin-kapt'`이 설정되어 있어야 합니다.

### Retrofit 등록:

```kotlin
Retrofit.Builder()
    .baseUrl(BASE_URL)
    .addConverterFactory(MoshiConverterFactory.create())
```

---

## 🆚 Gson vs Moshi 비교

| 항목        | Gson              | Moshi                |
| --------- | ----------------- | -------------------- |
| 지원 시기     | 오래됨 (성숙)          | 최신 (Kotlin 최적화)      |
| 어노테이션     | `@SerializedName` | `@Json`              |
| null 처리   | 제한적               | 안전                   |
| 성능        | 충분히 빠름            | 약간 더 빠름 (경우에 따라 다름)  |
| Kotlin 지원 | 약함 (Java 기반)      | 강함 (Kotlin-friendly) |
| 코드 생성     | 없음                | 어댑터 자동 생성 필요         |

---

## ✅ 어떤 걸 선택해야 할까?

| 상황                            | 추천 라이브러리              |
| ----------------------------- | --------------------- |
| 빠르게 시작하고 싶다                   | ✅ Gson (기본 포함, 설정 간단) |
| Kotlin 기반 프로젝트이며 타입 안정성이 중요하다 | ✅ Moshi               |
| 회사에서 이미 정해져 있다                | 조직 룰을 따름              |

---

## 📚 참고 링크

* [Gson 공식 문서](https://github.com/google/gson)
* [Moshi 공식 문서](https://github.com/square/moshi)
* [Retrofit 컨버터 비교](https://square.github.io/retrofit/)
