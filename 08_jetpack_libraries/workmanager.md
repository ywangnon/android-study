# WorkManager 기본 사용법 (Jetpack 백그라운드 작업 관리)

## ⏱️ WorkManager란?

WorkManager는 Android에서 **지연 실행, 반복 실행, 조건부 실행이 필요한 백그라운드 작업**을 처리할 수 있도록 도와주는 Jetpack 라이브러리입니다.

> 앱이 종료되거나 재부팅되어도 작업을 **자동으로 재시작**합니다.

---

## 🎯 WorkManager는 언제 쓰나?

- 서버 데이터 동기화
- 로컬 DB 백업
- 주기적인 리포트 전송
- 알림 예약
- 대용량 파일 업로드/다운로드

---

## 📦 Gradle 설정

```gradle
implementation "androidx.work:work-runtime-ktx:2.9.0"
````

> KTX 버전은 Coroutine을 지원하므로 더 간결한 코드 사용 가능

---

## 🧱 기본 사용 흐름

```
1. Worker 클래스 작성
2. 작업 요청 생성
3. WorkManager로 작업 예약
```

---

## 🧪 1. Worker 클래스 만들기

```kotlin
class UploadWorker(
    context: Context,
    workerParams: WorkerParameters
) : CoroutineWorker(context, workerParams) {

    override suspend fun doWork(): Result {
        // 백그라운드에서 실행할 작업
        Log.d("WorkManager", "작업 실행 중")
        delay(2000) // 예시: 2초 대기
        return Result.success()
    }
}
```

---

## 📤 2. 작업 요청 만들기

```kotlin
val uploadRequest = OneTimeWorkRequestBuilder<UploadWorker>().build()
```

> 반복 실행을 원할 경우 `PeriodicWorkRequestBuilder<>()` 사용

---

## ▶️ 3. 작업 예약 (한 번 실행)

```kotlin
WorkManager.getInstance(context)
    .enqueue(uploadRequest)
```

---

## ⚙️ 제약 조건 추가 예시

```kotlin
val constraints = Constraints.Builder()
    .setRequiresCharging(true)
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .build()

val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setConstraints(constraints)
    .build()
```

> 예: 충전 중이면서 네트워크 연결 시에만 작업 실행

---

## 🔁 반복 작업 예시

```kotlin
val repeatingRequest = PeriodicWorkRequestBuilder<UploadWorker>(
    15, TimeUnit.MINUTES
).build()

WorkManager.getInstance(context)
    .enqueueUniquePeriodicWork(
        "upload_job",
        ExistingPeriodicWorkPolicy.KEEP,
        repeatingRequest
    )
```

> 최소 간격은 Android 정책상 **15분**입니다.

---

## 🧾 작업 상태 추적

```kotlin
WorkManager.getInstance(context)
    .getWorkInfoByIdLiveData(request.id)
    .observe(this) { workInfo ->
        Log.d("Work", "상태: ${workInfo.state}")
    }
```

---

## ✅ WorkManager 요약

| 기능                    | 설명                 |
| --------------------- | ------------------ |
| `CoroutineWorker`     | 코루틴 기반 백그라운드 작업 처리 |
| `OneTimeWorkRequest`  | 한 번만 실행할 작업        |
| `PeriodicWorkRequest` | 주기적으로 반복할 작업       |
| `Constraints`         | 충전 중, 네트워크 등 조건 설정 |
| `enqueue()`           | 작업 실행 예약           |

---

## 📚 참고 자료

* [WorkManager 공식 문서](https://developer.android.com/topic/libraries/architecture/workmanager)
* [코루틴 기반 Worker 사용법](https://developer.android.com/topic/libraries/architecture/workmanager/how-to/define-work)
