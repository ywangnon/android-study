# 8. Jetpack 라이브러리 개요

Jetpack은 Android 앱 개발에 필요한 핵심 라이브러리들을 모아 놓은 **Android 공식 아키텍처 컴포넌트**입니다.

### 📦 Jetpack을 사용하는 이유

- 앱 아키텍처를 더 견고하고 유지보수 가능하게 함
- 수많은 기능을 직접 구현할 필요 없음 (Navigation, DB, 권한 등)
- Google 공식 지원 → 안정성 + 업데이트 지속

---

## 🧭 Navigation (네비게이션 컴포넌트)

**역할:** 앱 내 여러 화면(Composable/Fragment/Activity) 간의 전환과 백스택 관리를 쉽게 해줌.

### 주요 기능
- 화면 간 안전한 이동
- 인자 전달 (Safe Args)
- 하단 탭/드로어 Navigation UI와 통합 가능

---

## 🗃️ Room

**역할:** SQLite 기반의 로컬 데이터베이스를 더 쉽게 사용하도록 도와주는 ORM(Object Relational Mapping) 라이브러리

### 주요 기능
- SQL 쿼리를 작성하되 Kotlin/Java 객체로 결과를 다룸
- LiveData/Flow 연동으로 DB 변경 자동 반영
- 타입 변환, 관계 매핑, 쿼리 캐싱 등 지원

---

## 💾 DataStore

**역할:** SharedPreferences를 대체하는 Jetpack의 키-값 저장소

### 주요 기능
- 비동기 + 타입 안정성 + 코루틴 기반
- `Preferences DataStore` (Key-Value)
- `Proto DataStore` (Schema 기반)

> 기존 SharedPreferences보다 더 안전하고 테스트 가능함

---

## ⏱️ WorkManager

**역할:** 네트워크 요청, 백업 등 앱이 꺼져도 보장되어야 하는 백그라운드 작업을 예약 실행

### 주요 기능
- 일정 조건에서 실행 (예: 충전 중, Wi-Fi 연결 시)
- 앱이 종료되어도 백그라운드 실행 유지
- 연속 실행/지연 실행/제약 조건 설정

---

## ✅ 요약 정리

| 라이브러리 | 역할 | 비고 |
|------------|------|------|
| Navigation | 화면 간 이동 관리 | Compose/Fragment 모두 지원 |
| Room       | 로컬 DB 처리       | SQLite 기반 ORM |
| DataStore  | 설정값 저장소      | SharedPreferences 대체 |
| WorkManager | 백그라운드 작업 실행 | JobScheduler, AlarmManager 대체 |

---

## 📚 참고 자료

- [Android Jetpack 공식 소개](https://developer.android.com/jetpack)
- [Navigation 사용법](https://developer.android.com/guide/navigation)
- [Room 데이터베이스](https://developer.android.com/jetpack/androidx/releases/room)
- [DataStore 소개](https://developer.android.com/topic/libraries/architecture/datastore)
- [WorkManager 가이드](https://developer.android.com/topic/libraries/architecture/workmanager)
