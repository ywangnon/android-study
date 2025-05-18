# 10. 앱 배포와 빌드 관리

앱을 개발한 후 실제 기기에서 테스트하거나 Play Store에 업로드하려면 **APK 또는 AAB 빌드**, **서명**, **릴리즈 설정**이 필요합니다.

---

## ⚙️ 1. 빌드 시스템: Gradle

Android 앱은 **Gradle**을 사용하여 프로젝트를 빌드합니다.  
`build.gradle` 파일을 통해 **빌드 타입, 서명 설정, 의존성 관리** 등을 할 수 있습니다.

### 주요 파일

| 파일 | 설명 |
|------|------|
| `build.gradle (Project)` | 전체 프로젝트 설정 (Gradle 버전, 플러그인 등) |
| `build.gradle (Module)` | 앱 모듈별 빌드 설정 (앱 이름, 버전, 의존성 등) |

---

## 🏗️ 2. 빌드 타입: Debug vs Release

```gradle
android {
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
            versionNameSuffix "-debug"
        }
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}
````

| 빌드 타입   | 설명                   |
| ------- | -------------------- |
| Debug   | 개발자 테스트용, 로그 출력 O    |
| Release | 사용자 배포용, 최적화 및 서명 필요 |

---

## 🔐 3. 앱 서명 (Signing Config)

### 1) 서명 키 만들기 (key.jks)

```bash
keytool -genkey -v -keystore key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias mykey
```

### 2) build.gradle에 서명 설정 추가

```gradle
android {
    signingConfigs {
        release {
            storeFile file("key.jks")
            storePassword "your-store-password"
            keyAlias "mykey"
            keyPassword "your-key-password"
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

> 보안상 실제 비밀번호는 `local.properties`에 저장하고 참조하는 방식이 권장됩니다.

---

## 📦 4. APK vs AAB

| 포맷     | 설명                                                         |
| ------ | ---------------------------------------------------------- |
| `.apk` | 단일 설치 파일 (테스트/직접 배포용)                                      |
| `.aab` | Android App Bundle. Play Store에 업로드 시 사용 (자동으로 최적 APK 생성됨) |

---

## 🚀 5. APK / AAB 생성하기

### Android Studio 메뉴

```
Build > Build Bundle(s) / APK(s) > Build APK(s)
Build > Build Bundle(s) / APK(s) > Build Bundle
```

빌드가 완료되면 `app/build/outputs/apk/release/` 또는 `bundle/release/` 에 파일이 생성됩니다.

---

## ☁️ 6. Google Play에 앱 배포

1. Google Play Console 접속
2. 개발자 계정 등록 (연 25\$)
3. 앱 등록 → 패키지명 + 아이콘 + 설명 입력
4. 서명된 `.aab` 파일 업로드
5. 정책, 콘텐츠 등 검토 → 배포

> 첫 앱은 검토에 3\~7일 정도 걸릴 수 있습니다.

---

## 💡 릴리즈 팁

* `proguard-rules.pro`에 필요한 설정 추가 (로그 제거, 보안 강화 등)
* 버전 코드와 이름을 명확히 관리:

```gradle
defaultConfig {
    versionCode 2
    versionName "1.1.0"
}
```

* 앱 크기 줄이기: 이미지 최적화, 리소스 압축, obfuscation

---

## ✅ 요약

| 작업         | 도구/위치           |
| ---------- | --------------- |
| 빌드 설정      | build.gradle    |
| 릴리즈/디버그 구분 | buildTypes      |
| 서명 키 생성    | keytool         |
| APK/AAB 빌드 | Build 메뉴 또는 CLI |
| 배포         | Play Console    |

---

## 📚 참고 자료

* [App signing](https://developer.android.com/studio/publish/app-signing)
* [Build an Android App Bundle](https://developer.android.com/guide/app-bundle/build)
* [Play Console 시작하기](https://play.google.com/console)
