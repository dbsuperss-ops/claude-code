# Feature Development 플러그인

코드베이스 탐색, 아키텍처 설계, 품질 리뷰를 위한 특화 에이전트를 갖춘 기능 개발을 위한 종합적이고 구조화된 워크플로.

## 개요

Feature Development 플러그인은 새로운 기능을 구축하기 위한 체계적인 7단계 접근 방식을 제공합니다. 코드 작성으로 바로 뛰어드는 대신, 코드베이스 이해, 명확화 질문, 아키텍처 설계, 품질 보장 과정을 안내하여 기존 코드와 원활하게 통합되는 더 잘 설계된 기능을 만들어냅니다.

## 철학

기능을 구축하려면 코드 작성 이상이 필요합니다. 다음이 필요합니다:
- **변경 전 코드베이스 이해**
- **모호한 요구사항 명확화를 위한 질문**
- **구현 전 신중한 설계**
- **구축 후 품질 리뷰**

이 플러그인은 이러한 관행을 `/feature-dev` 명령어 사용 시 자동으로 실행되는 구조화된 워크플로에 내장합니다.

## 명령어: `/feature-dev`

7가지 별개의 단계로 구성된 가이드된 기능 개발 워크플로를 실행합니다.

**사용법:**
```bash
/feature-dev Add user authentication with OAuth
```

또는 간단히:
```bash
/feature-dev
```

명령어가 전체 과정을 대화형으로 안내합니다.

## 7단계 워크플로

### 1단계: 발견

**목표**: 무엇을 구축해야 하는지 이해

**수행 작업:**
- 불명확한 경우 기능 요청 명확화
- 어떤 문제를 해결하는지 질문
- 제약 사항과 요구사항 식별
- 이해한 내용 요약 및 확인

**예시:**
```
User: /feature-dev Add caching
Claude: Let me understand what you need...
        - What should be cached? (API responses, computed values, etc.)
        - What are your performance requirements?
        - Do you have a preferred caching solution?
```

### 2단계: 코드베이스 탐색

**목표**: 관련 기존 코드와 패턴 이해

**수행 작업:**
- 2-3개의 `code-explorer` 에이전트를 병렬로 실행
- 각 에이전트가 다른 측면 탐색 (유사한 기능, 아키텍처, UI 패턴)
- 에이전트가 읽을 주요 파일과 함께 포괄적인 분석 반환
- Claude가 식별된 모든 파일을 읽어 깊은 이해 구축
- 결과의 포괄적인 요약 제시

**실행되는 에이전트:**
- "[기능]과 유사한 기능을 찾아 구현을 추적"
- "[영역]의 아키텍처와 추상화를 매핑"
- "[관련 기능]의 현재 구현을 분석"

**예시 출력:**
```
Found similar features:
- User authentication (src/auth/): Uses JWT tokens, middleware pattern
- Session management (src/session/): Redis-backed, 24hr expiry
- API security (src/api/middleware/): Rate limiting, CORS

Key files to understand:
- src/auth/AuthService.ts:45 - Core authentication logic
- src/middleware/authMiddleware.ts:12 - Request authentication
- src/config/security.ts:8 - Security configuration
```

### 3단계: 명확화 질문

**목표**: 빈 공간을 채우고 모든 모호성 해소

**수행 작업:**
- 코드베이스 결과와 기능 요청 검토
- 불명확한 측면 식별:
  - 엣지 케이스
  - 오류 처리
  - 통합 지점
  - 하위 호환성
  - 성능 요구사항
- 정리된 목록으로 모든 질문 제시
- **계속 진행하기 전에 답변 대기**

**예시:**
```
Before designing the architecture, I need to clarify:

1. OAuth provider: Which OAuth providers? (Google, GitHub, custom?)
2. User data: Store OAuth tokens or just user profile?
3. Existing auth: Replace current auth or add alongside?
4. Sessions: Integrate with existing session management?
5. Error handling: How to handle OAuth failures?
```

**중요**: 이 단계는 설계를 시작하기 전에 아무것도 모호하지 않도록 보장합니다.

### 4단계: 아키텍처 설계

**목표**: 여러 구현 접근 방식 설계

**수행 작업:**
- 2-3개의 `code-architect` 에이전트를 다른 포커스로 실행:
  - **최소 변경**: 최소한의 변경, 최대한의 재사용
  - **클린 아키텍처**: 유지보수성, 우아한 추상화
  - **실용적인 균형**: 속도 + 품질
- 모든 접근 방식 검토
- 이 작업에 가장 잘 맞는 것에 대한 의견 형성
- 트레이드오프와 추천사항이 포함된 비교 제시
- **어떤 접근 방식을 선호하는지 질문**

**예시 출력:**
```
I've designed 3 approaches:

Approach 1: Minimal Changes
- Extend existing AuthService with OAuth methods
- Add new OAuth routes to existing auth router
- Minimal refactoring required
Pros: Fast, low risk
Cons: Couples OAuth to existing auth, harder to test

Approach 2: Clean Architecture
- New OAuthService with dedicated interface
- Separate OAuth router and middleware
- Refactor AuthService to use common interface
Pros: Clean separation, testable, maintainable
Cons: More files, more refactoring

Approach 3: Pragmatic Balance
- New OAuthProvider abstraction
- Integrate into existing AuthService
- Minimal refactoring, good boundaries
Pros: Balanced complexity and cleanliness
Cons: Some coupling remains

Recommendation: Approach 3 - gives you clean boundaries without
excessive refactoring, and fits your existing architecture well.

Which approach would you like to use?
```

### 5단계: 구현

**목표**: 기능 구축

**수행 작업:**
- 시작하기 전에 **명시적인 승인 대기**
- 이전 단계에서 식별된 모든 관련 파일 읽기
- 선택된 아키텍처에 따라 구현
- 코드베이스 관습을 엄격하게 따름
- 깔끔하고 잘 문서화된 코드 작성
- 진행하면서 todos 업데이트

**참고:**
- 승인 후에만 구현 시작
- 2단계에서 발견된 패턴 따르기
- 4단계에서 설계된 아키텍처 사용
- 지속적으로 진행 상황 추적

### 6단계: 품질 리뷰

**목표**: 코드가 단순하고, DRY하고, 우아하며, 기능적으로 올바른지 보장

**수행 작업:**
- 3개의 `code-reviewer` 에이전트를 다른 포커스로 병렬 실행:
  - **단순성/DRY/우아함**: 코드 품질 및 유지보수성
  - **버그/정확성**: 기능적 정확성 및 논리 오류
  - **관습/추상화**: 프로젝트 표준 및 패턴
- 결과 통합
- 가장 심각한 문제 식별
- **결과 제시 및 원하는 작업 질문**:
  - 지금 수정
  - 나중에 수정
  - 현재 상태로 진행
- 결정에 따라 문제 해결

**예시 출력:**
```
Code Review Results:

High Priority Issues:
1. Missing error handling in OAuth callback (src/auth/oauth.ts:67)
2. Memory leak: OAuth state not cleaned up (src/auth/oauth.ts:89)

Medium Priority:
1. Could simplify token refresh logic (src/auth/oauth.ts:120)
2. Consider extracting OAuth config validation

All tests pass. Code follows project conventions.

What would you like to do?
```

### 7단계: 요약

**목표**: 완료된 작업 문서화

**수행 작업:**
- 모든 todos를 완료로 표시
- 다음을 요약:
  - 구축된 내용
  - 주요 결정 사항
  - 수정된 파일
  - 제안된 다음 단계

**예시:**
```
Feature Complete: OAuth Authentication

What was built:
- OAuth provider abstraction supporting Google and GitHub
- OAuth routes and middleware integrated with existing auth
- Token refresh and session integration
- Error handling for all OAuth flows

Key decisions:
- Used pragmatic approach with OAuthProvider abstraction
- Integrated with existing session management
- Added OAuth state to prevent CSRF

Files modified:
- src/auth/OAuthProvider.ts (new)
- src/auth/AuthService.ts
- src/routes/auth.ts
- src/middleware/authMiddleware.ts

Suggested next steps:
- Add tests for OAuth flows
- Add more OAuth providers (Microsoft, Apple)
- Update documentation
```

## 에이전트

### `code-explorer`

**목적**: 실행 경로를 추적하여 기존 코드베이스 기능을 깊이 분석

**포커스 영역:**
- 진입점 및 호출 체인
- 데이터 흐름 및 변환
- 아키텍처 레이어 및 패턴
- 의존성 및 통합
- 구현 세부 사항

**트리거 시기:**
- 2단계에서 자동으로
- 코드 탐색 시 수동으로 호출 가능

**출력:**
- 파일:라인 참조가 있는 진입점
- 단계별 실행 흐름
- 주요 구성 요소 및 책임
- 아키텍처 인사이트
- 읽어야 할 필수 파일 목록

### `code-architect`

**목적**: 기능 아키텍처 및 구현 청사진 설계

**포커스 영역:**
- 코드베이스 패턴 분석
- 아키텍처 결정
- 컴포넌트 설계
- 구현 로드맵
- 데이터 흐름 및 빌드 순서

**트리거 시기:**
- 4단계에서 자동으로
- 아키텍처 설계를 위해 수동으로 호출 가능

**출력:**
- 발견된 패턴 및 관습
- 근거가 있는 아키텍처 결정
- 완전한 컴포넌트 설계
- 특정 파일이 있는 구현 맵
- 단계별 빌드 순서

### `code-reviewer`

**목적**: 버그, 품질 문제, 프로젝트 관습에 대한 코드 리뷰

**포커스 영역:**
- 프로젝트 가이드라인 준수 (CLAUDE.md)
- 버그 감지
- 코드 품질 문제
- 신뢰도 기반 필터링 (80 이상 높은 신뢰도 문제만 보고)

**트리거 시기:**
- 6단계에서 자동으로
- 코드 작성 후 수동으로 호출 가능

**출력:**
- 치명적 문제 (신뢰도 75-100)
- 중요 문제 (신뢰도 50-74)
- 파일:라인 참조가 있는 구체적인 수정 사항
- 프로젝트 가이드라인 참조

## 사용 패턴

### 전체 워크플로 (새 기능에 권장):
```bash
/feature-dev Add rate limiting to API endpoints
```

모든 7단계를 통해 워크플로가 안내합니다.

### 수동 에이전트 호출:

**기능 탐색:**
```
"Launch code-explorer to trace how authentication works"
```

**아키텍처 설계:**
```
"Launch code-architect to design the caching layer"
```

**코드 리뷰:**
```
"Launch code-reviewer to check my recent changes"
```

## 모범 사례

1. **복잡한 기능에는 전체 워크플로 사용**: 7단계가 철저한 계획을 보장
2. **명확화 질문에 신중하게 답변**: 3단계가 미래의 혼란을 방지
3. **아키텍처를 신중하게 선택**: 4단계가 이유가 있어 옵션을 제공
4. **코드 리뷰를 건너뛰지 않기**: 6단계가 프로덕션 전에 문제를 잡아냄
5. **제안된 파일 읽기**: 2단계가 주요 파일을 식별하므로 컨텍스트를 이해하기 위해 읽기

## 이 플러그인 사용 시기

**사용:**
- 여러 파일을 건드리는 새 기능
- 아키텍처 결정이 필요한 기능
- 기존 코드와의 복잡한 통합
- 요구사항이 다소 불명확한 기능

**사용하지 않을 때:**
- 한 줄 버그 수정
- 사소한 변경
- 잘 정의된 단순한 작업
- 긴급 핫픽스

## 요구사항

- Claude Code 설치
- Git 저장소 (코드 리뷰용)
- 기존 코드베이스가 있는 프로젝트 (워크플로는 학습할 기존 코드를 가정)

## 문제 해결

### 에이전트가 너무 오래 걸림

**문제**: 코드 탐색 또는 아키텍처 에이전트가 느림

**해결책**:
- 대용량 코드베이스에는 정상
- 에이전트가 가능한 경우 병렬로 실행
- 철저함이 더 나은 이해로 이어짐

### 너무 많은 명확화 질문

**문제**: 3단계에서 너무 많은 질문

**해결책**:
- 초기 기능 요청을 더 구체적으로
- 제약 사항에 대한 컨텍스트를 미리 제공
- 정말 선호도가 없다면 "최선이라고 생각하는 대로"라고 말하기

### 아키텍처 옵션이 압도적

**문제**: 4단계에서 너무 많은 아키텍처 옵션

**해결책**:
- 추천을 신뢰하세요 - 코드베이스 분석에 기반
- 여전히 불확실하다면 더 많은 설명 요청
- 의심스러울 때 실용적인 옵션 선택

## 팁

- **기능 요청을 구체적으로**: 더 많은 세부 사항 = 더 적은 명확화 질문
- **과정을 신뢰하기**: 각 단계가 이전 단계를 기반으로 구축
- **에이전트 출력 검토**: 에이전트가 코드베이스에 대한 귀중한 인사이트 제공
- **단계 건너뛰지 않기**: 각 단계는 목적이 있음
- **학습에 사용**: 탐색 단계가 자신의 코드베이스에 대해 가르쳐 줌

## 작성자

Sid Bidasaria (sbidasaria@anthropic.com)

## 버전

1.0.0
