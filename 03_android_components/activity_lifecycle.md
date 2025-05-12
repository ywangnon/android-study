# 3-2. Activity 생명주기 이해하기

## 🎯 Activity란?

`Activity`는 Android 앱에서 하나의 **화면(UI)을 담당하는 컴포넌트**입니다.  
사용자가 앱을 실행하면 가장 먼저 실행되는 기본 단위이며, 사용자의 상호작용을 처리하는 핵심 역할을 합니다.

---

## 🔄 생명주기(Lifecycle)란?

Activity는 사용자와의 상호작용 및 시스템 이벤트에 따라 **특정한 순서로 상태가 변화**합니다.  
이 상태 변화의 흐름을 `Activity 생명주기`라고 하며, Android 시스템이 자동으로 각 단계를 호출합니다.

---

## 🧭 주요 생명주기 콜백 메서드

| 메서드 | 설명 |
|--------|------|
| `onCreate()` | Activity가 생성될 때 호출. 초기화 작업 수행 |
| `onStart()` | 화면이 사용자에게 보이기 시작할 때 호출 |
| `onResume()` | 사용자와 상호작용이 가능한 상태가 되었을 때 호출 |
| `onPause()` | 다른 Activity가 나타나기 직전 호출. 리소스 정리 가능 |
| `onStop()` | 화면이 더 이상 보이지 않을 때 호출 |
| `onRestart()` | 중지되었다가 다시 시작될 때 호출 |
| `onDestroy()` | Activity가 완전히 종료될 때 호출 |

---

## 🔁 생명주기 흐름 다이어그램

```

onCreate()
↓
onStart()
↓
onResume()
↘
사용자와 상호작용 중
↙
onPause()
↓
onStop()
↓
onDestroy()

````

---

## 🧪 예제 코드

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d("LifeCycle", "onCreate() 호출됨")
    }

    override fun onStart() {
        super.onStart()
        Log.d("LifeCycle", "onStart() 호출됨")
    }

    override fun onResume() {
        super.onResume()
        Log.d("LifeCycle", "onResume() 호출됨")
    }

    override fun onPause() {
        super.onPause()
        Log.d("LifeCycle", "onPause() 호출됨")
    }

    override fun onStop() {
        super.onStop()
        Log.d("LifeCycle", "onStop() 호출됨")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("LifeCycle", "onDestroy() 호출됨")
    }
}
````

---

## 💡 활용 예시

* `onCreate()` → 화면 구성, 데이터 초기화
* `onPause()` → 음악 일시정지, 위치 서비스 중단
* `onDestroy()` → 메모리 해제, 리소스 정리

---

## 📚 참고 링크

* [공식 가이드: Activity 생명주기](https://developer.android.com/guide/components/activities/activity-lifecycle)
* [Android Developers Training](https://developer.android.com/training/basics/activity-lifecycle)
