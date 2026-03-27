# Claude Code

![](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square) [![npm]](https://www.npmjs.com/package/@anthropic-ai/claude-code)

[npm]: https://img.shields.io/npm/v/@anthropic-ai/claude-code.svg?style=flat-square

Claude Code는 터미널에서 실행되며, 코드베이스를 이해하고, 일상적인 작업 실행, 복잡한 코드 설명, git 워크플로 처리를 통해 더 빠르게 코딩할 수 있도록 도와주는 에이전틱 코딩 도구입니다 -- 모두 자연어 명령을 통해 가능합니다. 터미널, IDE에서 사용하거나 GitHub에서 @claude를 태그하세요.

**[공식 문서](https://code.claude.com/docs/en/overview)에서 자세히 알아보세요**.

<img src="./demo.gif" />

## 시작하기
> [!NOTE]
> npm을 통한 설치는 더 이상 사용되지 않습니다. 아래의 권장 방법 중 하나를 사용하세요.

더 많은 설치 옵션, 제거 단계 및 문제 해결은 [설정 문서](https://code.claude.com/docs/en/setup)를 참조하세요.

1. Claude Code 설치:

    **MacOS/Linux (권장):**
    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```

    **Homebrew (MacOS/Linux):**
    ```bash
    brew install --cask claude-code
    ```

    **Windows (권장):**
    ```powershell
    irm https://claude.ai/install.ps1 | iex
    ```

    **WinGet (Windows):**
    ```powershell
    winget install Anthropic.ClaudeCode
    ```

    **NPM (더 이상 사용되지 않음):**
    ```bash
    npm install -g @anthropic-ai/claude-code
    ```

2. 프로젝트 디렉토리로 이동하여 `claude`를 실행하세요.

## 플러그인

이 저장소에는 맞춤 명령과 에이전트로 기능을 확장하는 여러 Claude Code 플러그인이 포함되어 있습니다. 사용 가능한 플러그인에 대한 자세한 문서는 [플러그인 디렉토리](./plugins/README.md)를 참조하세요.

## 버그 보고

피드백을 환영합니다. Claude Code 내에서 `/bug` 명령을 사용하여 이슈를 직접 보고하거나 [GitHub 이슈](https://github.com/anthropics/claude-code/issues)를 제출하세요.

## Discord에서 연결하기

[Claude Developers Discord](https://anthropic.com/discord)에 참여하여 Claude Code를 사용하는 다른 개발자들과 연결하세요. 도움을 받고, 피드백을 공유하고, 커뮤니티와 프로젝트에 대해 논의하세요.

## 데이터 수집, 사용 및 보존

Claude Code를 사용하면 피드백을 수집하며, 여기에는 사용 데이터(예: 코드 수락 또는 거절), 관련 대화 데이터, `/bug` 명령을 통해 제출된 사용자 피드백이 포함됩니다.

### 데이터 사용 방법

[데이터 사용 정책](https://code.claude.com/docs/en/data-usage)을 참조하세요.

### 개인 정보 보호 장치

민감한 정보에 대한 제한된 보존 기간, 사용자 세션 데이터에 대한 제한된 접근, 모델 학습에 피드백을 사용하지 않는 명확한 정책 등 데이터를 보호하기 위한 여러 장치를 구현했습니다.

전체 세부 사항은 [상업 서비스 약관](https://www.anthropic.com/legal/commercial-terms) 및 [개인정보 처리방침](https://www.anthropic.com/legal/privacy)을 검토하세요.
