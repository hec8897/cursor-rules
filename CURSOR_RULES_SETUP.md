# Cursor Rules 전역 설정 가이드

이 폴더의 `.cursorrules` 파일을 전역 설정으로 사용하는 방법입니다.

## 방법 1: 홈 디렉토리에 복사 (권장)

1. 현재 폴더의 `.cursorrules` 파일을 홈 디렉토리로 복사합니다:

```bash
cp .cursorrules ~/.cursorrules
```

또는

```bash
cp /Users/dawoon/Desktop/dev/cursor-rull/.cursorrules ~/.cursorrules
```

2. Cursor를 재시작하면 전역 규칙이 적용됩니다.

## 방법 2: Cursor 설정에서 직접 설정

1. Cursor를 엽니다
2. `Cmd + ,` (Mac) 또는 `Ctrl + ,` (Windows/Linux)로 설정을 엽니다
3. "Cursor Rules" 또는 "Rules" 섹션을 찾습니다
4. `.cursorrules` 파일의 내용을 복사하여 붙여넣습니다

## 방법 3: 심볼릭 링크 사용 (고급)

여러 PC에서 동일한 규칙 파일을 공유하려면:

```bash
ln -s /Users/dawoon/Desktop/dev/cursor-rull/.cursorrules ~/.cursorrules
```

또는 클라우드 저장소(예: iCloud, Dropbox)에 저장하고 심볼릭 링크를 생성:

```bash
# 예: iCloud에 저장한 경우
ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.cursorrules ~/.cursorrules
```

## 규칙 우선순위

1. 프로젝트별 `.cursorrules` (프로젝트 루트)
2. 전역 `.cursorrules` (`~/.cursorrules`)
3. Cursor 기본 설정

프로젝트별 규칙이 전역 규칙보다 우선순위가 높습니다.

## 규칙 수정

규칙을 수정한 후:

- 프로젝트별 규칙: 해당 프로젝트에서만 적용
- 전역 규칙: 모든 프로젝트에 적용 (프로젝트별 규칙이 없는 경우)

## 확인 방법

Cursor에서 규칙이 적용되었는지 확인하려면:

1. Cursor 채팅에서 규칙에 대한 질문을 해보세요
2. 예: "현재 적용된 코딩 규칙을 알려주세요"

## 커밋 메시지 템플릿 설정

프로젝트에 포함된 `.gitmessage` 파일을 Git 커밋 템플릿으로 설정할 수 있습니다.

### 프로젝트별 설정

프로젝트 루트에서 다음 명령어를 실행합니다:

```bash
git config commit.template .gitmessage
```

### 전역 설정

모든 프로젝트에서 사용하려면:

```bash
# 템플릿 파일을 홈 디렉토리로 복사
cp .gitmessage ~/.gitmessage

# 전역 Git 설정에 추가
git config --global commit.template ~/.gitmessage
```

### 사용 방법

템플릿을 설정한 후 `git commit` 명령어를 실행하면 자동으로 템플릿이 열립니다:

```bash
git commit
```

템플릿의 주석 부분을 삭제하고 실제 커밋 메시지를 작성하면 됩니다.

자세한 내용은 `.github/COMMIT_TEMPLATE.md` 파일을 참고하세요.
