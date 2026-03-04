# Docker 및 Docker Compose 기초: 컨테이너로 환경 통합하기

애플리케이션을 개발할 때 가장 흔한 문제는 "내 컴퓨터에서는 잘 되는데 서버에서는 안 된다"는 것입니다. **Docker(도커)**는 애플리케이션과 그 실행 환경을 '컨테이너'라는 단위로 묶어 어디서든 동일하게 실행되도록 보장합니다.

---

## 1. Docker란 무엇인가?

Docker는 OS 수준의 가상화를 통해 프로세스를 격리된 환경(컨테이너)에서 실행하는 기술입니다. 
*   **이미지(Image):** 실행에 필요한 코드, 런타임, 설정 등을 담은 읽기 전용 템플릿입니다. (붕어빵 틀)
*   **컨테이너(Container):** 이미지를 실행한 상태입니다. 격리된 환경에서 독립적으로 동작합니다. (붕어빵)

---

## 2. Spring Boot 프로젝트 Dockerfile 작성하기

프로젝트 루트에 `Dockerfile`이라는 이름의 파일을 만들고 다음과 같이 작성합니다.

```dockerfile
# 1. 빌드 스테이지
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /app
COPY . .
RUN ./gradlew bootJar

# 2. 실행 스테이지 (이미지 최적화)
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 3. Docker Compose: 여러 컨테이너를 한 번에 관리하기

보통 웹 서비스는 '애플리케이션'과 '데이터베이스'가 함께 필요합니다. **Docker Compose**를 사용하면 `docker-compose.yml` 파일 하나로 여러 컨테이너를 동시에 제어할 수 있습니다.

### **docker-compose.yml 예시**
```yaml
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_DATABASE: mydb
      MYSQL_ROOT_PASSWORD: password
    ports:
      - "3306:3306"

  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://db:3306/mydb
```

*   **실행:** `docker-compose up -d` (백그라운드 실행)
*   **중지:** `docker-compose down`

---

## 4. 왜 Docker를 써야 할까?

1.  **환경 일치:** 개발자 PC, 테스트 서버, 운영 서버의 환경을 완벽히 일치시킵니다.
2.  **빠른 배포:** 이미지 전송만으로 서버 배포가 끝나며, 롤백도 매우 빠릅니다.
3.  **자원 효율성:** 가상머신(VM)보다 가볍고 빠르며 하나의 서버에 더 많은 서비스를 띄울 수 있습니다.

이제 Docker를 통해 복잡한 설치 과정 없이 클릭 한 번으로 서버 환경을 구축해 보세요!
