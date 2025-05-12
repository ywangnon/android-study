# 3. Android 기본 구성요소 정리

Android 앱은 여러 가지 핵심 컴포넌트로 구성됩니다.  
이 문서에서는 가장 중요한 구성요소 4가지인 `Activity`, `Fragment`, `View/ViewGroup`, `Intent`에 대해 설명합니다.

---

## 📱 Activity란?

- 하나의 **화면**을 담당하는 컴포넌트
- 사용자 인터페이스(UI)와 상호작용을 처리함
- 앱을 실행하면 가장 먼저 시작되는 기본 단위

### 대표 메서드
| 메서드 | 설명 |
|--------|------|
| `onCreate()` | Activity 생성 시 호출 |
| `onStart()` | 사용자에게 보이기 시작 |
| `onResume()` | 사용자와 상호작용 가능 |
| `onPause()` | 다른 화면 전환 직전 호출 |
| `onStop()` | Activity가 완전히 보이지 않음 |
| `onDestroy()` | 메모리에서 제거 시 호출 |

---

## 🧩 Fragment란?

- Activity 안에 포함되는 **재사용 가능한 UI 단위**
- 탭 UI, 페이지 슬라이드 등 복잡한 화면 구성이 필요한 경우에 사용
- Activity와 생명주기가 연결되어 있어 독립적이지 않음

### 사용 예
```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.container, SampleFragment())
    .commit()
````

---

## 🧱 View와 ViewGroup

### View란?

* Android에서 UI를 그리는 **기본 단위**
* 예: `TextView`, `Button`, `ImageView`

### ViewGroup이란?

* 여러 View를 묶어서 **레이아웃을 구성**하는 컨테이너 역할
* 예: `LinearLayout`, `ConstraintLayout`, `FrameLayout`

### 예시

```xml
<LinearLayout>
    <TextView android:text="이름" />
    <EditText android:hint="입력하세요" />
</LinearLayout>
```

---

## ✉️ Intent란?

* 컴포넌트 간 **데이터 전달 및 전환**을 담당하는 객체
* Activity → Activity 이동, 데이터 전달 등에 사용

### 명시적 Intent (명확한 대상 지정)

```kotlin
val intent = Intent(this, DetailActivity::class.java)
intent.putExtra("userId", 123)
startActivity(intent)
```

### 암시적 Intent (시스템에게 요청)

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://google.com"))
startActivity(intent)
```

---

## 🧭 Android 앱의 흐름 예시

```text
사용자 클릭
 → MainActivity 시작
   → MainFragment 로드
     → View와 ViewGroup으로 UI 구성
       → 버튼 클릭 시 Intent로 화면 이동
```

---

## 📚 참고 자료

* [Android 구성요소 공식 문서](https://developer.android.com/guide/components/fundamentals)
* [View System 소개](https://developer.android.com/guide/topics/ui/overview)
* [Intent 사용법](https://developer.android.com/guide/components/intents-filters)
