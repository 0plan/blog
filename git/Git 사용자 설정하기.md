# Git 사용자 설정하기

Git 사용자 설정은 사용자 이름과 이메일 주소를 설정하는 중요한 단계입니다. 이 정보는 커밋할 때마다 사용되며, 누가 코드를 작성했는지 식별하는 데 도움이 됩니다.

## 전역 사용자 설정

전역 설정은 시스템의 모든 Git 저장소에 적용됩니다:

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

이 명령어는 `~/.gitconfig` 파일에 설정을 저장합니다.

## 저장소별 사용자 설정

특정 프로젝트에 대해 다른 사용자 정보를 사용하려면:

```bash
git config user.name "Your Name"
git config user.email "youremail@example.com"
```

이 명령어는 해당 저장소의 `.git/config` 파일에 설정을 저장합니다.

## 설정 확인

현재 설정된 사용자 정보를 확인하려면:

```bash
git config user.name
git config user.email
```

전역 설정을 확인하려면 `--global` 옵션을 추가합니다.

## 주의사항

- 설정한 사용자 정보는 향후 커밋에만 적용되며, 이전 커밋의 정보는 변경되지 않습니다.
- GUI 도구를 사용하는 경우, 처음 실행 시 이 설정을 요구할 수 있습니다.
- 프로젝트마다 다른 사용자 정보를 사용하려면 `--global` 옵션을 제외하고 설정합니다.

사용자 설정은 Git을 사용하기 전에 반드시 해야 하는 중요한 단계입니다. 이를 통해 협업 시 누가 어떤 변경을 했는지 쉽게 파악할 수 있습니다.
