# GitHub Actions를 이용한 CI/CD 구축하기: 배포 자동화의 기초

개발자가 코드를 `Push`하면 자동으로 테스트를 실행하고 서버에 배포까지 해준다면 어떨까요? **GitHub Actions**는 이 과정을 자동화하는 **CI/CD(지속적 통합/지속적 제공)** 도구입니다. 

---

## 1. CI/CD란 무엇인가?

*   **CI (Continuous Integration):** 여러 개발자의 코드가 정기적으로 빌드되고 자동 테스트를 통과하도록 보장합니다.
*   **CD (Continuous Deployment):** 빌드된 코드를 실제 운영 서버에 자동으로 배포하여 사용자가 즉시 사용할 수 있게 합니다.

---

## 2. GitHub Actions 시작하기 (.github/workflows)

프로젝트 루트에 `.github/workflows/deploy.yml` 파일을 만들고 다음과 같이 작성합니다.

```yaml
name: Java CI/CD with Gradle

on:
  push:
    branches: [ "main" ] # 메인 브랜치에 푸시될 때만 실행

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    - name: Set up JDK 21
      uses: actions/setup-java@v3
      with:
        java-version: '21'
        distribution: 'temurin'

    - name: Build with Gradle
      run: ./gradlew build

    - name: Docker build & push
      run: |
        echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
        docker build -t my-app .
        docker tag my-app ${{ secrets.DOCKER_USERNAME }}/my-app:latest
        docker push ${{ secrets.DOCKER_USERNAME }}/my-app:latest

    - name: SSH Remote Commands (Deploy)
      uses: appleboy/ssh-action@v0.1.7
      with:
        host: ${{ secrets.SERVER_HOST }}
        username: ${{ secrets.SERVER_USER }}
        key: ${{ secrets.SERVER_KEY }}
        script: |
          docker pull ${{ secrets.DOCKER_USERNAME }}/my-app:latest
          docker stop my-app || true
          docker rm my-app || true
          docker run -d --name my-app -p 8080:8080 ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

---

## 3. 핵심 과정 이해하기

1.  **Checkout:** 현재 저장소의 코드를 GitHub Actions 실행기로 가져옵니다.
2.  **Build:** 코드를 빌드하고 테스트 케이스를 실행합니다. 에러가 나면 다음 단계로 가지 않고 멈춥니다.
3.  **Docker Push:** 빌드된 이미지를 Docker Hub와 같은 저장소에 업로드합니다.
4.  **Deploy:** 실제 운영 서버에 접속하여 새 이미지를 내려받고 실행 중인 컨테이너를 교체합니다.

---

## 4. 비밀 정보 관리 (Secrets)

서버의 주소, 비밀번호, SSH 키 등은 외부에 공개되면 안 됩니다.
*   **GitHub Repository Settings -> Secrets and variables -> Actions**에 가서 위 설정 파일에 적힌 `${{ secrets.XXX }}` 정보를 안전하게 등록해야 합니다.

이제 수동 배포의 번거로움과 실수에서 벗어나, 코드 작성에만 집중할 수 있는 자동화된 환경을 만들어 보세요!
