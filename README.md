# Cursor Rules

Cursor AI를 위한 체계적인 코딩 규칙 템플릿 및 가이드 모음입니다.

## 📋 프로젝트 목적

이 프로젝트는 Cursor AI와의 협업에서 일관된 코드 품질과 개발 워크플로우를 유지하기 위한 규칙 템플릿을 제공합니다.

주요 목적:

- **일관성 유지**: 프로젝트 전반에 걸쳐 일관된 코드 스타일과 패턴 유지
- **생산성 향상**: AI 어시스턴트가 프로젝트의 컨벤션을 이해하고 따르도록 가이드
- **품질 보장**: 코드 품질, 보안, 성능에 대한 명확한 가이드라인 제공
- **협업 효율화**: 팀원들이 공통된 규칙을 공유하여 코드 리뷰 및 협업 효율 향상

## 🎯 주요 기능

### 1. 구조화된 규칙 템플릿

- 계층적 구조로 구성된 `.cursorrules` 파일 템플릿
- 메타데이터, 핵심 원칙, 코드 스타일, 프로젝트 구조 등 체계적인 섹션 구성

### 2. 아키텍처 가이드

- Cursor Rules의 구조와 설계 원칙 설명
- 규칙 우선순위 및 모듈화 전략 가이드
- 모범 사례 및 활용 팁 제공

### 3. 설정 가이드

- 전역 설정 방법 안내
- 프로젝트별 설정 방법 안내
- 규칙 우선순위 및 적용 방법 설명

## 📁 프로젝트 구조

```
cursor-rule/
├── .cursorrules                    # Cursor AI 코딩 규칙 템플릿
├── .cursor/
│   └── rules/
│       ├── ui-rules/               # UI/프론트엔드 규칙
│       │   └── react.md           # React 코딩 규칙
│       └── workflow-rules/         # 워크플로우 규칙
│           └── pr.md               # PR 생성 규칙 (Git Flow)
├── .github/
│   ├── pull_request_template.md    # GitHub PR 템플릿
│   └── COMMIT_TEMPLATE.md          # 커밋 메시지 템플릿 가이드
├── .gitmessage                     # Git 커밋 메시지 템플릿
├── CURSOR_RULES_ARCHITECTURE.md   # 아키텍처 가이드 문서
├── CURSOR_RULES_SETUP.md          # 설정 가이드 문서
└── README.md                       # 프로젝트 소개 문서
```

## 🚀 시작하기

### 1. 프로젝트별 규칙 적용

프로젝트 루트 디렉토리에 `.cursorrules` 파일을 복사하거나 내용을 참고하여 커스터마이징합니다:

```bash
cp .cursorrules /path/to/your/project/.cursorrules
```

### 2. 전역 규칙 설정

모든 프로젝트에 공통으로 적용할 규칙을 설정하려면:

```bash
cp .cursorrules ~/.cursorrules
```

자세한 설정 방법은 [CURSOR_RULES_SETUP.md](./CURSOR_RULES_SETUP.md)를 참고하세요.

### 3. 규칙 커스터마이징

`.cursorrules` 파일을 프로젝트나 팀의 특성에 맞게 수정합니다:

- 기술 스택별 규칙 활성화/비활성화
- 프로젝트 특화 규칙 추가
- 팀 컨벤션 반영

## 📚 문서

- **[CURSOR_RULES_ARCHITECTURE.md](./CURSOR_RULES_ARCHITECTURE.md)**: 규칙의 구조와 설계 원칙에 대한 상세 가이드
- **[CURSOR_RULES_SETUP.md](./CURSOR_RULES_SETUP.md)**: 전역 및 프로젝트별 설정 방법 안내
- **[.cursor/rules/workflow-rules/pr.md](./.cursor/rules/workflow-rules/pr.md)**: Git Flow 기반 PR 생성 및 작성 가이드
- **[.cursor/rules/ui-rules/react.md](./.cursor/rules/ui-rules/react.md)**: React 코딩 규칙 가이드

## 🎨 규칙 구성

### 메인 규칙 파일 (`.cursorrules`)

`.cursorrules` 파일은 다음과 같은 섹션으로 구성됩니다:

- 📋 **메타데이터**: 버전 관리 및 목적 명시
- 🎯 **핵심 원칙**: 커뮤니케이션 및 코드 품질 기본 원칙
- 📝 **코드 스타일**: 네이밍 컨벤션 및 포맷팅 규칙
- 📁 **프로젝트 구조**: 파일 및 폴더 조직 가이드
- 🔒 **코드 품질**: 에러 처리, 성능, 보안 규칙
- 🧪 **개발 워크플로우**: 테스트, 문서화, PR 가이드
- 🔧 **기술 스택별 규칙**: TypeScript, React, Node.js 등
- 🤖 **AI 어시스턴트 지침**: Cursor AI 동작 가이드
- 🎨 **프로젝트별 커스터마이징**: 프로젝트 특화 규칙 공간

### 세부 규칙 파일

- **`.cursor/rules/ui-rules/react.md`**: React 전용 코딩 규칙
- **`.cursor/rules/workflow-rules/pr.md`**: Git Flow 기반 PR 생성 규칙
  - 브랜치 전략 및 네이밍 규칙
  - PR 제목 및 본문 작성 가이드
  - 커밋 메시지 규칙 (Conventional Commits)
  - PR 리뷰 및 병합 가이드라인
- **`.gitmessage`**: Git 커밋 메시지 템플릿 (Conventional Commits 형식)
- **`.github/COMMIT_TEMPLATE.md`**: 커밋 메시지 작성 가이드 및 예시

## 🔄 규칙 우선순위

1. **프로젝트별 규칙** (프로젝트 루트의 `.cursorrules`) - 최우선
2. **전역 규칙** (`~/.cursorrules`)
3. **Cursor 기본 설정**

## 💡 활용 팁

- **점진적 적용**: 모든 규칙을 한 번에 적용하지 말고, 필요한 것부터 추가
- **팀 협의**: 팀과 규칙을 공유하고 피드백 수집
- **정기 리뷰**: 분기별로 규칙을 검토하고 업데이트
- **예시 활용**: 복잡한 규칙은 예시 코드와 함께 작성

## 📝 라이선스

이 프로젝트는 자유롭게 사용하고 수정할 수 있습니다.

## 🤝 기여하기

규칙 개선 제안이나 버그 리포트는 이슈로 등록해주세요.

## 🗺️ 앞으로의 작업 계획

### 목표 구조

프로젝트를 다음과 같은 `.cursor/` 디렉토리 구조로 재구성할 예정입니다:

```
.cursor/
├── environment.json      # 환경별 설정 (개발/프로덕션 등)
├── modes.json           # Cursor 모드 설정 (코드 리뷰, 리팩토링 등)
└── rules/               # 규칙 모듈화 디렉토리
    ├── core-rules/      # 핵심 원칙 및 기본 규칙
    ├── my-rules/        # 개인/프로젝트 특화 규칙
    ├── global-rules/    # 전역 공통 규칙
    ├── testing-rules/   # 테스트 관련 규칙
    ├── tool-rules/      # 도구별 규칙 (ESLint, Prettier 등)
    ├── ts-rules/        # TypeScript 전용 규칙
    └── ui-rules/        # UI/프론트엔드 관련 규칙 (React, Vue, Svelte 등)
```

### 작업 단계

#### 1단계: 디렉토리 구조 설계 및 문서화

- [ ] 각 디렉토리의 역할과 책임 명확히 정의
- [ ] 규칙 모듈 간 의존성 및 우선순위 매핑
- [ ] 마이그레이션 가이드 문서 작성

#### 2단계: 설정 파일 설계

- [ ] `environment.json` 스키마 설계
  - 환경별 변수 설정 (개발/스테이징/프로덕션)
  - 프로젝트 타입별 설정 (웹앱/라이브러리/서버 등)
- [ ] `modes.json` 스키마 설계
  - Cursor 모드 정의 (코드 리뷰, 리팩토링, 신규 기능 개발 등)
  - 모드별 활성화 규칙 매핑

#### 3단계: 규칙 모듈화

- [ ] `core-rules/` 구성
  - 핵심 원칙 (커뮤니케이션, 코드 품질)
  - 기본 코드 스타일 가이드
- [ ] `global-rules/` 구성
  - 전역 공통 규칙 (에러 처리, 보안, 성능)
  - 프로젝트 구조 가이드
- [ ] `testing-rules/` 구성
  - 테스트 작성 가이드
  - 테스트 프레임워크별 규칙
- [ ] `tool-rules/` 구성
  - ESLint, Prettier 설정 가이드
  - Git 컨벤션 규칙
  - 빌드 도구 규칙
- [ ] `ts-rules/` 구성
  - TypeScript 타입 정의 규칙
  - 타입 안전성 가이드
  - 제네릭 및 유틸리티 타입 사용법
- [ ] `ui-rules/` 구성
  - **React 규칙 분리 작업** (`react/` 폴더 구조)
    - [ ] `react/` 디렉토리 생성 및 구조 설계
    - [ ] `react/README.md` 작성
      - React 규칙 전체 개요 및 인덱스
      - 각 규칙 파일의 역할과 사용 가이드
      - 규칙 파일 간 의존성 및 참조 관계 설명
    - [ ] `react/code-quality.md` 작성
      - 좋은 코드를 위한 4가지 기준 (토스 프론트엔드 펀더멘털 기반)
      - 가독성: 맥락 줄이기, 이름 붙이기, 위에서 아래로 읽히게 하기
      - 예측 가능성: 이름 관리, 반환 타입 통일, 숨은 로직 드러내기
      - 응집도: 함께 수정되는 파일 배치, 매직 넘버 제거, 폼 응집도
      - 결합도: 단일 책임, 중복 코드 허용, Props Drilling 제거
      - 코드 품질의 트레이드오프 고려사항
    - [ ] `react/components.md` 작성
      - 컴포넌트 설계 원칙 및 구조
      - Props 설계 및 타입 정의
      - 컴포넌트 분리 기준 및 패턴
      - 컴포넌트 구성 순서 및 네이밍 컨벤션
    - [ ] `react/hooks.md` 작성
      - 기본 Hooks 규칙 (useState, useEffect 등)
      - useMemo & useCallback 사용 가이드
      - 커스텀 Hooks 설계 원칙
      - Hooks 패턴 및 베스트 프랙티스
    - [ ] `react/state-management.md` 작성
      - 로컬 상태 vs 전역 상태 판단 기준
      - Context API 사용 가이드
      - 상태 관리 라이브러리 패턴 (Redux, Zustand 등)
      - 서버 상태 관리 (React Query, SWR 등)
    - [ ] `react/performance.md` 작성
      - 리렌더링 최적화 전략
      - 리스트 렌더링 최적화
      - 코드 스플리팅 및 지연 로딩
      - 이미지 최적화 가이드
      - 성능 프로파일링 방법
    - [ ] `react/patterns.md` 작성
      - 컴포넌트 패턴 (Container/Presentational, Compound Components 등)
      - 렌더링 패턴 (Conditional Rendering, List Rendering 등)
      - 이벤트 처리 패턴
      - 폼 처리 패턴
      - 에러 처리 패턴 (Error Boundary 등)
    - [ ] `react/testing.md` 작성
      - React Testing Library 사용 가이드
      - 컴포넌트 테스트 원칙
      - Hooks 테스트 방법
      - 통합 테스트 및 E2E 테스트 가이드
    - [ ] `react/accessibility.md` 작성
      - 접근성 기본 원칙 (a11y)
      - ARIA 속성 사용 가이드
      - 키보드 네비게이션 지원
      - 포커스 관리 및 시맨틱 HTML
    - [ ] 기존 `react.md` 파일을 위 구조로 분리 및 마이그레이션
  - Vue, Svelte, Angular 등 다른 프레임워크 규칙
  - 공통 UI/프론트엔드 규칙
    - 스타일링 가이드 (CSS-in-JS, CSS Modules 등)
    - 공통 접근성 가이드
- [ ] `my-rules/` 구성
  - 프로젝트 특화 규칙 템플릿
  - 팀 컨벤션 예시

#### 4단계: 통합 및 참조 시스템 구축

- [ ] 규칙 모듈 간 참조 메커니즘 설계
- [ ] 우선순위 및 병합 전략 정의
- [ ] 규칙 로딩 및 적용 로직 문서화

#### 5단계: 마이그레이션 도구 개발

- [ ] 기존 `.cursorrules` 파일을 새 구조로 변환하는 스크립트
- [ ] 규칙 검증 도구
- [ ] 규칙 적용 테스트 도구

#### 6단계: 문서화 및 예시

- [ ] 각 규칙 모듈별 README 작성
- [ ] 사용 예시 및 베스트 프랙티스 문서화
- [ ] FAQ 및 트러블슈팅 가이드

#### 7단계: 테스트 및 검증

- [ ] 다양한 프로젝트 타입에서 테스트
- [ ] 규칙 충돌 및 우선순위 검증
- [ ] 성능 및 로딩 시간 최적화

### 예상 이점

- **모듈화**: 규칙을 기능별로 분리하여 관리 용이성 향상
- **재사용성**: 필요한 규칙만 선택적으로 적용 가능
- **확장성**: 새로운 규칙 카테고리 추가 용이
- **유지보수성**: 특정 영역의 규칙만 수정하여 영향 범위 최소화
- **협업**: 팀원들이 각자의 규칙을 `my-rules/`에 추가하여 공유 가능
- **React 규칙 분리의 이점**:
  - 각 파일이 적절한 크기로 유지되어 가독성 향상
  - 특정 주제(예: Hooks, 성능 최적화)만 빠르게 찾아서 참조 가능
  - 규칙 추가/수정 시 해당 파일만 수정하여 충돌 최소화
  - 프로젝트별로 필요한 React 규칙만 선택적으로 적용 가능

### 고려사항

- 기존 `.cursorrules` 파일과의 호환성 유지
- 규칙 로딩 순서 및 우선순위 명확화
- Cursor AI의 실제 동작 방식과의 호환성 확인 필요
- 문서화의 중요성 (각 모듈의 역할과 사용법)
- **React 규칙 분리 시 고려사항**:
  - 규칙 파일 간 참조 및 의존성 관리
  - 중복된 내용을 피하면서도 각 파일의 독립성 유지
  - `react/README.md`를 통한 전체 구조 및 사용법 안내
  - 프로젝트별로 필요한 React 규칙만 선택적으로 로드하는 메커니즘

---

**버전**: 1.0.0  
**마지막 업데이트**: 2024
