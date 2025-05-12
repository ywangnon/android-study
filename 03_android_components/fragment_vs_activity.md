# 3-2. Activity와 Fragment의 차이점

## 🤔 왜 Fragment가 등장했을까?

초기의 Android 앱은 모든 화면을 `Activity`로 구성했습니다.  
하지만 복잡한 UI 구성, 다양한 화면 크기(예: 태블릿), 재사용 가능한 UI 단위를 만들기 위해 `Fragment`라는 개념이 도입되었습니다.

---

## ⚔️ Activity vs Fragment 비교

| 항목 | Activity | Fragment |
|------|----------|----------|
| 개념 | 화면 전체를 담당하는 독립적 컴포넌트 | Activity 내부에서 사용되는 UI 조각 |
| 생명주기 | 시스템에 의해 직접 관리 | Activity의 생명주기에 종속적 |
| 재사용성 | 제한적 (하나의 Activity = 하나의 UI) | 여러 Activity 또는 레이아웃에서 재사용 가능 |
| UI 구성 | 단독으로 UI 구성 | 다른 Fragment 또는 Activity 안에 포함 |
| 전환 | `Intent` 사용 | `FragmentTransaction`을 통해 전환 |
| 독립성 | 높은 독립성 | Activity에 의존 |

---

## 🧱 언제 Activity를 쓰고 언제 Fragment를 쓸까?

### ✅ Activity를 쓰기 좋은 경우
- 앱의 **전체 흐름**을 담당할 때
- 별도의 **기능 단위**를 명확히 구분하고 싶을 때
- 깊은 Activity 전환이 필요하지 않을 때

### ✅ Fragment를 쓰기 좋은 경우
- 탭 UI, Bottom Navigation 등 **부분 화면 전환**이 자주 필요한 경우
- 태블릿 / 폴더블 등 **멀티패널 UI** 지원이 필요할 때
- 동일한 UI 조각을 **재사용**하고자 할 때

---

## 💡 함께 사용하는 예

보통은 하나의 Activity 안에 여러 Fragment를 넣고,
- 화면 상단: 공통 Toolbar
- 화면 본문: Fragment 교체로 콘텐츠 변경
이런 식의 구조를 사용합니다.

```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.container, SampleFragment())
    .commit()
````

---

## 📚 참고 자료

* [Fragment 공식 가이드](https://developer.android.com/guide/fragments)
* [Activity와 Fragment의 설계 기준](https://developer.android.com/guide/components/fragments#design)
