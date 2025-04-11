![Flutter 로고](https://upload.wikimedia.org/wikipedia/commons/1/17/Google-flutter-logo.png)
# 플러터(Flutter)란?
[Flutter](https://flutter.io/)는 Google에서 개발한 오픈소스 UI 프레임워크로, 하나의 코드베이스를 통해 **Android**, **iOS**, **Windows**, **macOS**, **Linux**, 그리고 웹 플랫폼에서 작동하는 애플리케이션을 개발할 수 있습니다. 
이 프레임워크는 빠른 개발, 높은 성능, 그리고 플랫폼 간 일관된 사용자 경험을 제공하는 데 초점을 맞추고 있습니다.

## Flutter의 주요 특징

1. **크로스 플랫폼 개발**  
    Flutter는 단일 코드베이스로 여러 플랫폼에서 실행 가능한 애플리케이션을 개발할 수 있어 개발 비용과 시간을 절약할 수 있습니다.
    
2. **Dart 언어 사용**  
    Flutter는 Google에서 개발한 프로그래밍 언어인 Dart를 사용합니다. Dart는 자바와 파이썬, C와 유사한 친숙한 문법을 제공하며 배우기 쉽고 강력합니다.

3. **위젯 기반 구조**  
    Flutter의 모든 UI 구성 요소는 **위젯**으로 구성됩니다. 위젯은 화면의 레이아웃, 스타일, 애니메이션 등을 정의하며, 이를 조합하여 복잡한 UI를 만들 수 있습니다.
    
4. **독립적인 렌더링 엔진**  
    Flutter는 Skia라는 고성능 렌더링 엔진을 사용하여 운영체제에 의존하지 않고 독립적으로 UI를 렌더링합니다. 이를 통해 네이티브 수준의 성능과 일관된 디자인을 제공합니다.
    
5. **빠른 개발 속도**  
    Flutter는 **Hot Reload** 기능을 통해 코드 변경 사항을 실시간으로 반영하여 개발 속도를 크게 향상시킵니다.

## Flutter의 아키텍처

Flutter는 세 가지 주요 레이어로 구성됩니다:

1. **Embedder Layer**  
    각 플랫폼별로 운영체제와 상호작용하며 엔트리 포인트를 제공합니다. 이를 통해 사용자 입력, 통신, 렌더링 등의 기능을 수행합니다.
    
2. **Engine Layer**  
    Skia 렌더링 엔진과 저수준 API를 포함하며, C++로 작성되어 높은 성능을 제공합니다. 이 레이어는 Dart 코드를 실행하고 UI를 렌더링합니다.
    
3. **Framework Layer**  
    Dart로 작성된 고수준 API를 제공하며, 애니메이션, 제스처, 레이아웃 등의 기능을 지원합니다. Material 및 Cupertino 디자인 시스템도 포함되어 있어 다양한 스타일의 앱 제작이 가능합니다.
    

## Flutter의 장점

- **빠른 퍼포먼스**: 독립적인 렌더링 엔진으로 인해 네이티브 앱에 가까운 성능 제공.
    
- **개발 생산성 향상**: Hot Reload와 크로스 플랫폼 지원으로 개발 시간 단축.
    
- **풍부한 위젯 라이브러리**: 다양한 UI 구성 요소 및 디자인 시스템 제공.
    
- **커뮤니티와 지원**: Google의 강력한 지원과 활발한 오픈소스 커뮤니티 활동.

## 사용해보기
Flutter로 앱을 개발하기 위해 필요한 환경 설정과 기본적인 사용법을 단계별로 알아보겠습니다.

### 1. 개발 환경 설정

#### a. Flutter SDK 설치
1. [Flutter 공식 웹사이트](https://flutter.dev/)에 접속하여 SDK를 다운로드합니다.
2. 압축을 풀고, 원하는 위치에 Flutter 폴더를 두세요.
3. 시스템 환경 변수에 Flutter의 `bin` 디렉토리를 추가합니다.

#### b. IDE 설치
- Flutter는 여러 IDE와 호환되지만, **Android Studio** 또는 **Visual Studio Code**를 추천합니다. 둘 다 Flutter와 Dart 플러그인을 설치하여 사용할 수 있습니다.

### 2. Flutter 프로젝트 생성

1. 터미널(또는 명령 프롬프트)을 열고 아래 명령어를 입력하여 새로운 Flutter 프로젝트를 생성합니다:
```bash
flutter create my_app
   ```
(여기서 `my_app`은 프로젝트 이름입니다.)

2. 생성된 프로젝트 폴더로 이동합니다:
```bash
cd my_app
   ```

### 3. 앱 실행

1. 에뮬레이터를 실행하거나 실제 기기를 연결합니다.
2. 아래 명령어로 앱을 실행합니다:
```bash
flutter run
   ```

### 4. 기본 코드 구조 이해하기

`lib/main.dart` 파일이 Flutter 앱의 시작점입니다. 기본적으로 제공되는 코드는 아래와 같습니다:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter Demo Home Page'),
        ),
        body: Center(
          child: Text('Hello, Flutter!'),
        ),
      ),
    );
  }
}
```

### 5. 위젯 추가 및 수정

- Flutter는 위젯 기반으로 작동합니다. 기존 코드를 수정하여 UI를 변경해보세요.
- 예를 들어, `Text('Hello, Flutter!')`를 다른 텍스트로 변경하거나, 새로운 위젯을 추가할 수 있습니다.

### 6. Hot Reload 기능 활용

코드를 수정한 후, 에뮬레이터에서 `r` 키를 눌러 Hot Reload를 실행하면, 변경 사항이 즉시 반영됩니다. 이를 통해 빠르게 UI를 수정하고 테스트할 수 있습니다.

### 7. 패키지 추가

- Flutter의 다양한 패키지를 활용하고 싶다면 `pubspec.yaml` 파일에 패키지를 추가한 후, 아래 명령어로 설치합니다:
```bash
flutter pub get
  ```

### 마무리
이제 기본적인 Flutter 사용 방법을 익혔습니다. Flutter의 다양한 기능을 활용하여 자신만의 앱을 만들어 보세요!
