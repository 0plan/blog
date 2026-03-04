# Java Swing 프로젝트 시작하기: 첫 번째 윈도우 띄우기

자바 스윙(Java Swing)은 JDK에 기본 포함되어 있어 별도의 복잡한 라이브러리 설치 없이 바로 시작할 수 있다는 것이 큰 장점입니다. 2026년 기준 최신 JDK 환경에서도 동일하게 작동하는 기본적인 스윙 프로젝트 설정과 첫 실행 코드를 작성해 보겠습니다.

---

## 1. 개발 환경 준비

*   **JDK:** 최신 LTS 버전(Java 17, 21, 25 등)이 설치되어 있어야 합니다.
*   **IDE:** IntelliJ IDEA, Eclipse, 혹은 VS Code 등 어떤 도구든 상관없습니다.
*   **빌드 도구 (선택 사항):** Maven이나 Gradle을 사용하면 프로젝트 관리가 훨씬 쉬워집니다.

---

## 2. 프로젝트 생성 (Maven 기준)

가장 권장되는 방식인 **Maven** 프로젝트를 생성하고, `pom.xml`에 자바 버전을 설정합니다. (Swing은 기본 라이브러리이므로 별도의 `dependency`를 추가할 필요가 없습니다.)

```xml
<properties>
    <maven.compiler.source>21</maven.compiler.source>
    <maven.compiler.target>21</maven.compiler.target>
</properties>
```

---

## 3. 첫 번째 Swing 코드 작성

`Main.java` 파일을 생성하고 아래 코드를 작성합니다. 스윙 코드를 작성할 때 가장 중요한 것은 **EDT(Event Dispatch Thread)**를 통해 UI를 실행하는 것입니다.

### **HelloSwing.java**

```java
import javax.swing.*;

public class HelloSwing {
    public static void main(String[] args) {
        // UI 작업을 EDT(Event Dispatch Thread)에서 실행하도록 요청
        SwingUtilities.invokeLater(() -> {
            createAndShowGUI();
        });
    }

    private static void createAndShowGUI() {
        // 1. 윈도우 프레임 생성
        JFrame frame = new JFrame("My First Swing App");
        
        // 2. 닫기 버튼을 눌렀을 때 프로그램도 종료되도록 설정
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        // 3. 윈도우 크기 설정 (가로, 세로)
        frame.setSize(400, 300);

        // 4. 컴포넌트 추가 (라벨 하나 추가)
        JLabel label = new JLabel("Hello, Java Swing!", SwingConstants.CENTER);
        frame.add(label);

        // 5. 윈도우를 화면 중앙에 배치
        frame.setLocationRelativeTo(null);

        // 6. 윈도우를 화면에 표시
        frame.setVisible(true);
    }
}
```

---

## 4. 코드 설명: 주요 컴포넌트 분석

### **JFrame**
*   애플리케이션의 메인 윈도우를 나타내는 클래스입니다. 제목 표시줄, 최소화/최대화 버튼, 닫기 버튼 등을 포함합니다.
*   `setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)`: 이 설정이 없으면 윈도우를 닫아도 백그라운드에서 자바 프로세스가 계속 실행되므로 반드시 추가해야 합니다.

### **SwingUtilities.invokeLater(...)**
*   스윙의 모든 UI 작업은 전용 스레드(EDT)에서 이루어져야 합니다. 이 메서드를 사용하면 메인 스레드에서 생성된 작업을 안전하게 EDT로 넘겨 실행할 수 있습니다.

### **JLabel**
*   화면에 텍스트나 이미지를 표시할 때 사용하는 가장 기본적인 컴포넌트입니다.

---

## 5. 실행 및 확인

코드를 실행하면 중앙에 "Hello, Java Swing!"이라는 글자가 적힌 400x300 크기의 윈도우가 팝업됩니다.

이제 여기에 버튼(`JButton`), 입력창(`JTextField`), 체크박스(`JCheckBox`) 등을 추가하며 기능을 확장해 보세요. 각각의 컴포넌트는 `frame.add()`를 통해 화면에 붙일 수 있으며, 레이아웃 매니저(`LayoutManager`)를 사용하면 더 정교하게 배치할 수 있습니다.

### **다음 단계 제안**
*   **이벤트 처리:** 버튼을 클릭했을 때 알림창(`JOptionPane`) 띄우기.
*   **레이아웃:** `BorderLayout`이나 `FlowLayout` 이해하기.
*   **데이터 연동:** 입력창에 쓴 글자를 라벨로 옮기기.

이제 당신의 첫 번째 자바 데스크톱 앱이 완성되었습니다!
