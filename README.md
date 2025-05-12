## 📚 Android 개발 정리 개요

이 저장소는 Android 개발을 처음 시작하는 개발자를 위한 학습 기록입니다. Android Studio, Kotlin, Jetpack Compose 등 현대적인 Android 개발 흐름을 따라 실습과 이론을 함께 정리합니다.
주요 목적은 다음과 같습니다:

* **개념 정리**: Android 개발에 필요한 핵심 이론을 정리
* **실습 기록**: 작은 실습 프로젝트를 통해 실제 코드 작성 경험 쌓기
* **문제 해결**: 개발 중 겪는 오류와 해결 방법 기록
* **기술 확장**: Jetpack, Compose, MVVM 등 실무 기술에 대한 학습

---

## 🗂️ Android 개발 목차

### 1. Android 개발 시작하기

* Android란?
* Android 개발 환경 구성 (Android Studio 설치, SDK 설정)
* Kotlin vs Java: 왜 Kotlin인가?
* 첫 번째 프로젝트 만들기 (Hello Compose)

### 2. Kotlin 언어 기초

* 변수, 함수, 조건문, 반복문
* 클래스와 객체
* 람다와 고차 함수
* Null Safety

### 3. Android 기본 구성요소

* Activity란?
* Fragment란?
* View와 ViewGroup
* Intent와 데이터 전달

### 4. Jetpack Compose 기초

* Compose란?
* Composable 함수 구조
* 상태 (State) 관리
* Layout과 Modifier

### 5. 화면 구성 실습

* To-Do 앱 만들기
* 입력 폼과 리스트
* 체크박스와 삭제 기능
* ViewModel 연동 (MVVM 패턴 기초)

### 6. 앱 구조화와 MVVM

* MVVM 패턴 소개
* ViewModel과 LiveData/Rx/StateFlow
* Repository 패턴 기초
* DI(Hilt/Dagger) 소개

### 7. 네트워크 & API 연동

* Retrofit을 이용한 API 호출
* Gson/Moshi로 JSON 파싱
* 비동기 처리 (Coroutine)

### 8. Jetpack 라이브러리 정리

* Navigation Component
* Room (로컬 데이터베이스)
* WorkManager
* DataStore (SharedPreferences 대체)

### 9. 테스팅과 디버깅

* Logcat 사용법
* 단위 테스트 (JUnit)
* UI 테스트 (Espresso, Compose Testing)

### 10. 배포와 빌드 관리

* 앱 서명과 APK 생성
* Gradle 기초
* Play Store 배포 절차

---

## 📁 Git 저장소 구조 예시

```bash
android-study/
├── 01_android_intro/
│   └── what_is_android.md
├── 02_kotlin_basics/
│   └── kotlin_functions.md
├── 03_android_components/
│   └── activity_lifecycle.md
├── 04_jetpack_compose/
│   └── composable_functions.md
├── 05_sample_projects/
│   └── todo_app/
│       ├── README.md
│       └── screenshots/
├── 06_mvvm_architecture/
│   └── viewmodel_state.md
└── README.md
```
