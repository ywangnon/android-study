# Android 입문 학습 정리

이 저장소는 Android 개발을 처음 시작하는 분들을 위해 구성된 학습 가이드입니다.  
**Jetpack Compose 기반 실습**, **MVVM 아키텍처**, **네트워크 연동**, **앱 배포까지** 전 과정을 차근차근 배울 수 있습니다.

---

## 📚 목차

### 1. Android 개발 개요
- [Android란 무엇인가?](01_android_intro/android_overview.md)
- [Android 개발 환경과 툴 소개](01_android_intro/dev_environment.md)

### 2. Kotlin 기초
- [변수, 조건문, 함수, 클래스](02_kotlin_basics/kotlin_basics.md)

### 3. Android 기본 구성요소
- [Activity / Fragment / View / Intent 이해](03_android_components/android_components_overview.md)
- [Activity와 Fragment 차이점](03_android_components/fragment_vs_activity.md)

### 4. Jetpack Compose 기초
- [Composable 함수와 상태 관리](04_jetpack_compose/composable_basics.md)

### 5. Compose 실습 프로젝트
- [To-Do 앱 실습 (ViewModel + Compose)](05_sample_projects/todo_app/README.md)

### 6. 앱 구조화와 MVVM
- [MVVM 구조 개요](06_mvvm_architecture/mvvm_overview.md)
- [ViewModel과 상태 관리 (StateFlow vs LiveData)](06_mvvm_architecture/viewmodel_state.md)
- [Repository 패턴 기초](06_mvvm_architecture/repository_pattern.md)
- [DI(Hilt) 소개와 사용법](06_mvvm_architecture/di_hilt_intro.md)

### 7. 네트워크 & API 연동
- [Retrofit과 API 구조 이해](07_networking/networking_intro.md)
- [Retrofit 실습: To-Do API 연동](07_networking/retrofit_example.md)
- [Gson vs Moshi - JSON 파싱](07_networking/json_parsing.md)
- [Coroutine으로 비동기 처리](07_networking/coroutines.md)

### 8. Jetpack 라이브러리
- [Jetpack 개요 및 주요 라이브러리 정리](08_jetpack_libraries/overview.md)
- [Navigation (Compose 기반 화면 전환)](08_jetpack_libraries/navigation_compose.md)
- [Room - 로컬 DB 저장](08_jetpack_libraries/room_database.md)
- [DataStore - 설정 값 저장소](08_jetpack_libraries/datastore.md)
- [WorkManager - 백그라운드 작업 처리](08_jetpack_libraries/workmanager.md)

### 9. 테스팅과 디버깅
- [테스트 기초 & 로그/디버깅](09_testing_debugging/testing_basics.md)
- [Compose UI 테스트 & ViewModel 테스트](09_testing_debugging/compose_and_viewmodel_test.md)

### 10. 앱 배포와 빌드 관리
- [Gradle, APK/AAB 빌드, Play Store 업로드](10_build_release/build_basics.md)

---

## 🛠️ 환경

- Android Studio Hedgehog (2023.x 이상)
- Kotlin 1.9+
- Jetpack Compose 1.6.x
- Gradle 8.x

---

## 📌 목표

이 저장소를 통해 다음을 달성할 수 있습니다:

- Kotlin과 Jetpack Compose 기초 이해
- MVVM 아키텍처로 앱 구조화
- API와 로컬 DB 연동
- 테스트/디버깅/배포까지 전체 개발 사이클 경험

---

## ✨ 추천 실습

- 기존 To-Do 앱에 DataStore/Room 기능 확장
- WorkManager로 주기적 알림 기능 추가
- Retrofit 기반 뉴스/영화/날씨 앱 만들어보기

---

## 📬 기여

피드백, 이슈, PR 모두 환영합니다.  
여러분의 의견으로 이 저장소는 더 좋아집니다.
