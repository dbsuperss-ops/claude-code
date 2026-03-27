# Claude Code Plugins

이 디렉토리에는 커스텀 명령어, 에이전트, 워크플로를 통해 기능을 확장하는 공식 Claude Code 플러그인이 포함되어 있습니다. 이것들은 Claude Code 플러그인 시스템으로 가능한 것의 예시이며—커뮤니티 마켓플레이스를 통해 더 많은 플러그인을 사용할 수 있습니다.

## Claude Code 플러그인이란?

Claude Code 플러그인은 커스텀 슬래시 명령어, 특화된 에이전트, 훅, MCP 서버로 Claude Code를 향상시키는 확장 도구입니다. 플러그인은 프로젝트와 팀 간에 공유할 수 있으며, 일관된 도구와 워크플로를 제공합니다.

[공식 플러그인 문서](https://docs.claude.com/en/docs/claude-code/plugins)에서 자세히 알아보세요.

## 이 디렉토리의 플러그인

| 이름 | 설명 | 내용 |
|------|------|------|
| [agent-sdk-dev](./agent-sdk-dev/) | Claude Agent SDK 작업을 위한 개발 키트 | **명령어:** `/new-sdk-app` - 새 Agent SDK 프로젝트를 위한 대화형 설정<br>**에이전트:** `agent-sdk-verifier-py`, `agent-sdk-verifier-ts` - 모범 사례에 따른 SDK 애플리케이션 검증 |
| [claude-opus-4-5-migration](./claude-opus-4-5-migration/) | Sonnet 4.x 및 Opus 4.1에서 Opus 4.5로 코드와 프롬프트 마이그레이션 | **스킬:** `claude-opus-4-5-migration` - 모델 문자열, 베타 헤더, 프롬프트 조정의 자동화된 마이그레이션 |
| [code-review](./code-review/) | 거짓 양성을 필터링하는 신뢰도 기반 점수를 사용하는 여러 특화 에이전트를 이용한 자동화된 PR 코드 리뷰 | **명령어:** `/code-review` - 자동화된 PR 리뷰 워크플로<br>**에이전트:** CLAUDE.md 준수, 버그 감지, 히스토리 컨텍스트, PR 이력, 코드 댓글을 위한 5개의 병렬 Sonnet 에이전트 |
| [commit-commands](./commit-commands/) | 커밋, 푸시, 풀 리퀘스트 생성을 위한 Git 워크플로 자동화 | **명령어:** `/commit`, `/commit-push-pr`, `/clean_gone` - 간소화된 git 작업 |
| [explanatory-output-style](./explanatory-output-style/) | 구현 선택 및 코드베이스 패턴에 대한 교육적 인사이트를 추가 (더 이상 사용되지 않는 Explanatory 출력 스타일을 모방) | **훅:** SessionStart - 각 세션 시작 시 교육적 컨텍스트 주입 |
| [feature-dev](./feature-dev/) | 구조화된 7단계 접근 방식을 갖춘 종합적인 기능 개발 워크플로 | **명령어:** `/feature-dev` - 가이드된 기능 개발 워크플로<br>**에이전트:** `code-explorer`, `code-architect`, `code-reviewer` - 코드베이스 분석, 아키텍처 설계, 품질 리뷰를 위해 |
| [frontend-design](./frontend-design/) | 일반적인 AI 미학을 피하는 독특하고 프로덕션 수준의 프론트엔드 인터페이스 생성 | **스킬:** `frontend-design` - 프론트엔드 작업에 자동 호출, 대담한 디자인 선택, 타이포그래피, 애니메이션, 시각적 세부 사항에 대한 가이드 제공 |
| [hookify](./hookify/) | 대화 패턴이나 명시적 지시를 분석하여 원하지 않는 동작을 방지하는 커스텀 훅을 쉽게 생성 | **명령어:** `/hookify`, `/hookify:list`, `/hookify:configure`, `/hookify:help`<br>**에이전트:** `conversation-analyzer` - 문제가 있는 동작에 대한 대화 분석<br>**스킬:** `writing-rules` - hookify 규칙 문법 가이드 |
| [learning-output-style](./learning-output-style/) | 결정 시점에서 의미 있는 코드 기여를 요청하는 대화형 학습 모드 (미출시 Learning 출력 스타일을 모방) | **훅:** SessionStart - 교육적 인사이트를 받으면서 결정 시점에서 의미 있는 코드(5-10줄)를 작성하도록 사용자 장려 |
| [plugin-dev](./plugin-dev/) | 7개의 전문 스킬과 AI 지원 생성으로 Claude Code 플러그인 개발을 위한 종합 툴킷 | **명령어:** `/plugin-dev:create-plugin` - 플러그인 구축을 위한 8단계 가이드 워크플로<br>**에이전트:** `agent-creator`, `plugin-validator`, `skill-reviewer`<br>**스킬:** 훅 개발, MCP 통합, 플러그인 구조, 설정, 명령어, 에이전트, 스킬 개발 |
| [pr-review-toolkit](./pr-review-toolkit/) | 댓글, 테스트, 오류 처리, 타입 설계, 코드 품질, 코드 단순화를 전문으로 하는 종합 PR 리뷰 에이전트 | **명령어:** `/pr-review-toolkit:review-pr` - 선택적 리뷰 측면(comments, tests, errors, types, code, simplify, all)과 함께 실행<br>**에이전트:** `comment-analyzer`, `pr-test-analyzer`, `silent-failure-hunter`, `type-design-analyzer`, `code-reviewer`, `code-simplifier` |
| [ralph-wiggum](./ralph-wiggum/) | 반복적인 개발을 위한 대화형 자기참조 AI 루프. Claude는 완료될 때까지 동일한 작업을 반복적으로 작업 | **명령어:** `/ralph-loop`, `/cancel-ralph` - 자율 반복 루프 시작/중지<br>**훅:** Stop - 반복을 계속하기 위해 종료 시도를 가로챔 |
| [security-guidance](./security-guidance/) | 파일 편집 시 잠재적인 보안 문제에 대해 경고하는 보안 알림 훅 | **훅:** PreToolUse - 명령어 인젝션, XSS, eval 사용, 위험한 HTML, pickle 역직렬화, os.system 호출을 포함한 9가지 보안 패턴 모니터링 |

## 설치

이 플러그인들은 Claude Code 저장소에 포함되어 있습니다. 자신의 프로젝트에서 사용하려면:

1. Claude Code를 전역으로 설치:
```bash
npm install -g @anthropic-ai/claude-code
```

2. 프로젝트로 이동하여 Claude Code 실행:
```bash
claude
```

3. `/plugin` 명령어를 사용하여 마켓플레이스에서 플러그인을 설치하거나, 프로젝트의 `.claude/settings.json`에서 구성하세요.

자세한 플러그인 설치 및 구성은 [공식 문서](https://docs.claude.com/en/docs/claude-code/plugins)를 참조하세요.

## 플러그인 구조

각 플러그인은 표준 Claude Code 플러그인 구조를 따릅니다:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json          # 플러그인 메타데이터
├── commands/                # 슬래시 명령어 (선택적)
├── agents/                  # 특화된 에이전트 (선택적)
├── skills/                  # 에이전트 스킬 (선택적)
├── hooks/                   # 이벤트 핸들러 (선택적)
├── .mcp.json                # 외부 도구 구성 (선택적)
└── README.md                # 플러그인 문서
```

## 기여

이 디렉토리에 새 플러그인을 추가할 때:

1. 표준 플러그인 구조를 따르세요
2. 포괄적인 README.md를 포함하세요
3. `.claude-plugin/plugin.json`에 플러그인 메타데이터를 추가하세요
4. 모든 명령어와 에이전트를 문서화하세요
5. 사용 예시를 제공하세요

## 자세히 알아보기

- [Claude Code 문서](https://docs.claude.com/en/docs/claude-code/overview)
- [플러그인 시스템 문서](https://docs.claude.com/en/docs/claude-code/plugins)
- [Agent SDK 문서](https://docs.claude.com/en/api/agent-sdk/overview)
