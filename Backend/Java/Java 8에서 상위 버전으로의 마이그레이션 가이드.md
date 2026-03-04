# Java 8에서 상위 버전으로의 마이그레이션 가이드 (Workflow)

Java 8에서 11, 17, 혹은 21로 버전을 올리는 것은 단순한 숫자 변경 이상의 의미를 갖습니다. 특히 Java 9에서 도입된 모듈 시스템(Project Jigsaw)과 삭제된 API들로 인해 체계적인 접근이 필요합니다. 성공적인 마이그레이션을 위한 6단계 워크플로우를 정리합니다.

---

## 1단계: 사전 영향도 분석 (jdeps 활용)

가장 먼저 할 일은 우리 코드가 JDK 내부 API(Internal API)를 사용하고 있는지 확인하는 것입니다.

*   **`jdeps` 도구 사용:** JDK에 내장된 이 도구는 의존성을 분석해 줍니다.
    ```bash
    jdeps --jdk-internals -recursive -cp "lib/*" your-app.jar
    ```
*   **조치:** `sun.*` 또는 `com.sun.*` 계열의 내부 API 호출이 발견되면, `java.*` 또는 `javax.*`와 같은 표준 API로 대체해야 합니다. (예: `sun.misc.BASE64Encoder` → `java.util.Base64`)

---

## 2단계: 빌드 도구 및 라이브러리 업데이트

최신 Java를 지원하지 않는 빌드 도구와 라이브러리는 마이그레이션의 가장 큰 걸림돌입니다.

*   **빌드 도구 최신화:**
    *   **Maven:** 3.8.x 이상 권장
    *   **Gradle:** 7.3 이상 (Java 17/21 완벽 지원을 위해 8.x 권장)
*   **필수 라이브러리 업데이트:** 바이트코드를 조작하거나 리플렉션을 사용하는 라이브러리는 반드시 업데이트해야 합니다.
    *   Lombok (1.18.20+), Mockito (3.x+), Byte Buddy, Hibernate, Spring Boot (3.x는 Java 17+ 필수)

---

## 3단계: 삭제된 모듈 대응 (J2EE 모듈)

Java 11부터 JDK에 포함되어 있던 Java EE(현재 Jakarta EE) 관련 모듈이 삭제되었습니다.

*   **문제:** JAXB, JAX-WS, JTA 등이 포함된 코드는 컴파일 에러가 발생합니다.
*   **해결:** `pom.xml` 또는 `build.gradle`에 명시적으로 의존성을 추가해야 합니다.
    ```xml
    <!-- 예: JAXB 추가 -->
    <dependency>
        <groupId>jakarta.xml.bind</groupId>
        <artifactId>jakarta.xml.bind-api</artifactId>
        <version>3.0.1</version>
    </dependency>
    ```

---

## 4단계: 컴파일러 설정 변경 (Release 옵션)

단순히 `source`와 `target` 버전을 바꾸는 것보다 `--release` 옵션을 사용하는 것이 더 안전합니다.

*   **Maven 설정 예시:**
    ```xml
    <properties>
        <maven.compiler.release>17</maven.compiler.release>
    </properties>
    ```
*   **이유:** `release` 옵션은 해당 버전의 문법뿐만 아니라, 해당 버전에 존재하는 API인지까지 체크해 줍니다.

---

## 5단계: 런타임 및 리플렉션 오류 해결

Java 17은 JDK 내부 패키지를 강력하게 캡슐화합니다. 만약 외부 라이브러리가 리플렉션으로 JDK 내부를 건드린다면 `InaccessibleObjectException`이 발생할 수 있습니다.

*   **임시 조치:** 라이브러리 업데이트가 불가능할 경우 JVM 옵션으로 강제 개방할 수 있습니다.
    ```bash
    --add-opens java.base/java.lang=ALL-UNNAMED
    ```
*   **GC 로그 변경:** 기존의 `-XX:+PrintGCDetails` 같은 옵션은 `-Xlog:gc`와 같은 통합 로깅 시스템 옵션으로 대체해야 합니다.

---

## 6단계: 자동화 도구 활용 (OpenRewrite)

수백 개의 파일을 일일이 수정하기 어렵다면 자동화 도구를 고려해 보세요.

*   **OpenRewrite:** Java 마이그레이션 레시피를 제공하여 코드를 자동으로 리팩토링해 줍니다.
*   **실행 예시 (Maven):**
    ```bash
    mvn org.openrewrite.maven:rewrite-maven-plugin:run \
      -Drewrite.activeRecipes=org.openrewrite.java.migrate.UpgradeToJava17
    ```

---

## 마무리: 점진적 도입 전략

1.  **컴파일은 Java 8 타겟으로, 실행은 최신 JDK로:** 먼저 환경만 바꾸고 테스트를 통과시킵니다.
2.  **컴파일 타겟 상향:** 빌드 설정을 상향하고 컴파일 에러를 잡습니다.
3.  **새로운 기능 도입:** 성공적으로 마이그레이션이 끝난 후, `Record`, `Text Blocks`, `Switch Expressions` 같은 새로운 문법을 점진적으로 적용합니다.

마이그레이션은 고통스러운 과정일 수 있지만, Java 17/21이 주는 압도적인 성능 향상과 생산성 도구들은 그 가치가 충분합니다.
