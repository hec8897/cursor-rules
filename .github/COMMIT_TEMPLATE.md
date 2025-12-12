# 커밋 메시지 템플릿 가이드

이 파일은 커밋 메시지 작성 시 참고용 가이드입니다.

## 기본 형식 (Conventional Commits)

```
<타입>(<범위>): <제목>

<본문>

<푸터>
```

## 타입 (Type)

### 주요 타입

- **feat**: 새로운 기능 추가
- **fix**: 버그 수정
- **docs**: 문서 수정
- **style**: 코드 포맷팅, 세미콜론 누락 등 (코드 변경 없음)
- **refactor**: 코드 리팩토링
- **test**: 테스트 코드 추가 또는 수정
- **chore**: 빌드 업무 수정, 패키지 매니저 설정 등
- **perf**: 성능 개선
- **ci**: CI 설정 변경

## 범위 (Scope) - 선택사항

변경된 모듈이나 파일의 범위를 명시합니다.

**예시:**
- `auth`: 인증 관련
- `api`: API 관련
- `components`: 컴포넌트 관련
- `utils`: 유틸리티 함수
- `config`: 설정 파일

## 제목 작성 가이드

- **길이**: 50자 이내
- **형식**: 명확하고 간결하게
- **동사**: 과거형 사용하지 않음 ("추가했습니다" ❌ → "추가" ✅)
- **대소문자**: 첫 글자는 대문자로 시작

## 본문 작성 가이드

- **길이**: 72자마다 줄바꿈
- **내용**: "무엇을"과 "왜"를 설명
- **형식**: 불릿 포인트 사용 권장

## 푸터 작성 가이드

- **이슈 번호**: `Closes #123`, `Related to #456`
- **BREAKING CHANGE**: 주요 변경사항이 있는 경우

## 예시

### 간단한 커밋

```
feat: 사용자 로그인 기능 추가
```

### 범위 포함

```
feat(auth): JWT 토큰 인증 구현
```

### 본문 포함

```
feat: 사용자 프로필 페이지 추가

- 프로필 이미지 업로드 기능
- 사용자 정보 수정 기능
- 비밀번호 변경 기능

Closes #123
```

### BREAKING CHANGE 포함

```
feat: 결제 시스템 통합

- 결제 API 연동
- 결제 내역 조회 기능
- 결제 실패 처리 로직

BREAKING CHANGE: 결제 관련 API 엔드포인트 변경
```

## Git 템플릿 설정 방법

### 프로젝트별 설정

```bash
git config commit.template .gitmessage
```

### 전역 설정

```bash
git config --global commit.template ~/.gitmessage
```

템플릿 파일을 홈 디렉토리로 복사:

```bash
cp .gitmessage ~/.gitmessage
git config --global commit.template ~/.gitmessage
```

## 참고

- [Conventional Commits](https://www.conventionalcommits.org/)
- [.cursor/rules/workflow-rules/pr.md](../.cursor/rules/workflow-rules/pr.md) - PR 생성 규칙 참고

