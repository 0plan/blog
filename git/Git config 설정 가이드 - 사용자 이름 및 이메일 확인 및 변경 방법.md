# Git config 설정 가이드: 사용자 이름(name) 및 이메일(email) 확인 및 변경 방법

Git을 처음 설치하거나 새 프로젝트를 시작할 때 가장 먼저 확인해야 할 것이 바로 **사용자 설정(git config)**입니다. 커밋 메시지에 남는 작성자 정보는 협업의 기초이기 때문입니다.

이 포스트에서는 사용자들이 가장 많이 검색하는 **Git 사용자 이름 및 이메일 설정, 확인, 변경 방법**을 정리했습니다.

---

## 1. Git 사용자 정보 확인하기 (git config 확인)

현재 설정된 사용자 이름과 이메일이 무엇인지 먼저 확인해야 합니다.

### 전역(Global) 설정 확인
시스템 전체에 적용된 설정을 확인하려면 `--global` 옵션을 사용합니다.
```bash
# 전역 사용자 이름 확인
git config --global user.name

# 전역 사용자 이메일 확인
git config --global user.email

# 모든 전역 설정 리스트 확인
git config --global --list
```

### 특정 저장소(Local) 설정 확인
현재 작업 중인 프로젝트 폴더 내의 설정을 확인합니다. (프로젝트마다 다른 계정을 쓸 때 유용합니다.)
```bash
git config user.name
git config user.email
```

---

## 2. Git 사용자 이름 및 이메일 설정하기 (git config 설정)

### 전역 사용자 설정 (시스템 전체 적용)
가장 일반적으로 사용하는 방식입니다. 한 번 설정하면 모든 저장소에서 기본값으로 사용됩니다.
```bash
git config --global user.name "내이름"
git config --global user.email "my-email@example.com"
```
*이 정보는 `~/.gitconfig` 파일에 저장됩니다.*

### 특정 프로젝트별 사용자 설정 (Local)
회사 업무용 계정과 개인용 계정을 구분해서 사용해야 할 때, 해당 프로젝트 폴더 안에서 실행합니다.
```bash
git config user.name "업무용이름"
git config user.email "work-email@company.com"
```
*이 정보는 해당 저장소의 `.git/config` 파일에 저장됩니다.*

---

## 3. 설정된 사용자 정보 변경 및 삭제하기

### 정보 변경 방법
설정할 때와 동일한 명령어를 입력하면 기존 정보가 덮어씌워집니다.
```bash
# 이메일 주소를 변경하고 싶을 때
git config --global user.email "new-email@example.com"
```

### 설정 삭제 (Unset)
잘못 설정된 정보를 완전히 삭제하고 싶을 때 사용합니다.
```bash
git config --global --unset user.name
git config --global --unset user.email
```

---

## 자주 묻는 질문 (FAQ)

**Q. 커밋을 이미 했는데 작성자 정보가 틀렸어요. 어떻게 바꾸나요?**
A. 이미 완료된 커밋의 작성자 정보를 바꾸는 것은 조금 복잡합니다. 가장 최근 커밋만 수정하려면 다음 명령어를 사용하세요:
`git commit --amend --author="Name <email@address.com>"`

**Q. `git config --list`를 쳤는데 정보가 너무 많이 나와요.**
A. 설정이 중복되어 보일 수 있지만, Git은 **Local(저장소) > Global(전역) > System(시스템)** 순서의 우선순위를 가집니다. 프로젝트 폴더 내에 설정이 있다면 그것이 우선 적용됩니다.

---

**관련 검색어:** `git config username`, `git email 설정`, `git global config 확인`, `git 사용자 설정`, `git 계정 확인`
