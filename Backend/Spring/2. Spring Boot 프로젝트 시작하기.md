# Spring Boot 프로젝트 시작하기: 첫 번째 REST API 만들기

스프링 부트(Spring Boot)는 설정을 최소화하고 프로젝트를 신속하게 시작할 수 있도록 설계되었습니다. 2026년 기준 최신 환경인 **Java 21/25**와 **Spring Boot 3.x**를 사용하여 첫 번째 백엔드 애플리케이션을 만들어 보겠습니다.

---

## 1. 프로젝트 생성: Spring Initializr 활용

가장 빠르고 표준적인 방법은 [start.spring.io](https://start.spring.io) 서비스를 이용하는 것입니다.

### **설정 권장값:**
*   **Project:** Maven Project (혹은 Gradle - 취향에 따라 선택)
*   **Language:** Java
*   **Spring Boot:** 3.4.x (안정 버전 중 최신)
*   **Java:** 21 (LTS 버전 권장)
*   **Dependencies:** `Spring Web`, `Lombok`, `Spring Boot DevTools` 추가

설정 후 **GENERATE** 버튼을 눌러 ZIP 파일을 다운로드하고, 사용 중인 IDE(IntelliJ 등)에서 프로젝트를 엽니다.

---

## 2. 첫 번째 컨트롤러 작성

사용자의 요청을 받아 응답을 보내주는 가장 기본적인 API를 작성해 봅니다.

### **HelloController.java**

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring Boot 3 with Java 21!";
    }
}
```

*   **@RestController:** 이 클래스가 REST API를 제공하는 컨트롤러임을 명시합니다.
*   **@GetMapping("/hello"):** 브라우저에서 `/hello` 경로로 접속했을 때 해당 메서드를 실행하도록 매핑합니다.

---

## 3. 애플리케이션 실행

`Application.java` 파일을 열어 메인 메서드를 실행합니다. 스프링 부트에는 내장 서버(Tomcat)가 포함되어 있으므로, 별도의 서버 설치 없이 바로 실행됩니다.

실행 후 브라우저에서 `http://localhost:8080/hello`에 접속하여 "Hello, Spring Boot 3 with Java 21!"이라는 문구가 나오면 성공입니다.

---

## 4. 2026년 환경에 맞춘 추가 팁

### **성능 최적화: 가상 스레드 활성화**
Java 21/25 환경이라면 `application.properties`에 아래 한 줄을 추가하는 것만으로 처리 능력을 대폭 높일 수 있습니다.

```properties
spring.threads.virtual.enabled=true
```

### **Lombok 활용하기**
클래스에 `@Data`나 `@AllArgsConstructor` 같은 어노테이션을 붙여 Getter/Setter 등 반복적인 코드를 제거하세요.

---

## 5. 다음 단계 제안

*   **데이터베이스 연동:** Spring Data JPA를 사용하여 DB와 데이터를 주고받아 보세요.
*   **예외 처리:** `@RestControllerAdvice`를 사용하여 전역 에러 핸들링을 구현해 보세요.
*   **보안:** Spring Security를 통해 사용자 인증 및 인가 기능을 추가해 보세요.

이제 여러분은 강력한 자바 백엔드 개발의 첫발을 내디뎠습니다!
