<h1 align="center">Awesome Claude Skills</h1>

<p align="center">
<a href="https://composio.dev/?utm_source=Github&utm_medium=Youtube&utm_campaign=2025-11&utm_content=AwesomeSkills">
  <img width="1280" height="640" alt="Composio banner" src="https://github.com/user-attachments/assets/adb3f57a-2706-4329-856f-059a32059d48">
</a>


</p>

<p align="center">
  <a href="https://awesome.re">
    <img src="https://awesome.re/badge.svg" alt="Awesome" />
  </a>
  <a href="https://makeapullrequest.com">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs Welcome" />
  </a>
  <a href="https://www.apache.org/licenses/LICENSE-2.0">
    <img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat-square" alt="License: Apache-2.0" />
  </a>
</p>
<div>
<p align="center">
  <a href="https://twitter.com/composio">
    <img src="https://img.shields.io/badge/Follow on X-000000?style=for-the-badge&logo=x&logoColor=white" alt="Follow on X" />
  </a>
  <a href="https://www.linkedin.com/company/104100957">
    <img src="https://img.shields.io/badge/Follow on LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="Follow on LinkedIn" />
  </a>
  <a href="https://discord.com/invite/composio">
    <img src="https://img.shields.io/badge/Join our Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Join our Discord" />
  </a>
  </p>
</div>

Claude.ai, Claude Code 및 Claude API에서 생산성을 향상시키는 실용적인 Claude Skills의 엄선된 목록입니다.


> 스킬이 500개 이상의 앱에서 작동하도록 하려면 [Composio](https://composio.dev/?utm_source=Github&utm_medium=Youtube&utm_campaign=2025-11&utm_content=AwesomeSkills)와 연결하세요


## 목차

- [Claude Skills란 무엇인가?](#claude-skills란-무엇인가)
- [스킬](#스킬)
  - [문서 처리](#문서-처리)
  - [개발 및 코드 도구](#개발-및-코드-도구)
  - [데이터 및 분석](#데이터-및-분석)
  - [비즈니스 및 마케팅](#비즈니스-및-마케팅)
  - [커뮤니케이션 및 글쓰기](#커뮤니케이션-및-글쓰기)
  - [크리에이티브 및 미디어](#크리에이티브-및-미디어)
  - [생산성 및 조직](#생산성-및-조직)
  - [협업 및 프로젝트 관리](#협업-및-프로젝트-관리)
  - [보안 및 시스템](#보안-및-시스템)
- [시작하기](#시작하기)
- [스킬 만들기](#스킬-만들기)
- [기여하기](#기여하기)
- [리소스](#리소스)
- [라이선스](#라이선스)

## Claude Skills란 무엇인가?

Claude Skills는 특정 작업을 고유한 요구 사항에 따라 수행하는 방법을 Claude에게 가르치는 맞춤형 워크플로우입니다. Skills를 사용하면 Claude가 모든 Claude 플랫폼에서 반복 가능하고 표준화된 방식으로 작업을 실행할 수 있습니다.

## 스킬

### 문서 처리

- [docx](https://github.com/anthropics/skills/tree/main/document-skills/docx) - 변경 내용 추적, 주석, 서식을 사용하여 Word 문서를 생성, 편집, 분석합니다.
- [pdf](https://github.com/anthropics/skills/tree/main/document-skills/pdf) - 텍스트, 표, 메타데이터를 추출하고 PDF를 병합 및 주석 처리합니다.
- [pptx](https://github.com/anthropics/skills/tree/main/document-skills/pptx) - 슬라이드, 레이아웃, 템플릿을 읽고, 생성하고, 조정합니다.
- [xlsx](https://github.com/anthropics/skills/tree/main/document-skills/xlsx) - 스프레드시트 조작: 수식, 차트, 데이터 변환.
- [Markdown to EPUB Converter](https://github.com/smerchek/claude-epub-skill) - 마크다운 문서와 채팅 요약을 전문적인 EPUB 전자책 파일로 변환합니다. *제작: [@smerchek](https://github.com/smerchek)*

### 개발 및 코드 도구

- [artifacts-builder](https://github.com/anthropics/skills/tree/main/artifacts-builder) - 최신 프론트엔드 웹 기술(React, Tailwind CSS, shadcn/ui)을 사용하여 정교한 다중 구성 요소 claude.ai HTML 아티팩트를 생성하는 도구 모음입니다.
- [aws-skills](https://github.com/zxkane/aws-skills) - CDK 모범 사례, 비용 최적화 MCP 서버, 서버리스/이벤트 중심 아키텍처 패턴을 사용한 AWS 개발.
- [Changelog Generator](./changelog-generator/) - git 커밋에서 사용자 대상 체인지로그를 자동으로 생성하고, 기술적 커밋을 고객 친화적인 릴리스 노트로 변환합니다.
- [Claude Code Terminal Title](https://github.com/bluzername/claude-code-terminal-title) - 각 Claude Code 터미널 창에 수행 중인 작업을 설명하는 동적 제목을 제공하여 어떤 창이 무엇을 하고 있는지 추적할 수 있도록 합니다.
- [D3.js Visualization](https://github.com/chrisvoncsefalvay/claude-d3js-skill) - Claude가 D3 차트와 인터랙티브 데이터 시각화를 생성하도록 가르칩니다. *제작: [@chrisvoncsefalvay](https://github.com/chrisvoncsefalvay)*
- [FFUF Web Fuzzing](https://github.com/jthack/ffuf_claude_skill) - ffuf 웹 퍼저를 통합하여 Claude가 퍼징 작업을 실행하고 취약점에 대한 결과를 분석할 수 있습니다. *제작: [@jthack](https://github.com/jthack)*
- [finishing-a-development-branch](https://github.com/obra/superpowers/tree/main/skills/finishing-a-development-branch) - 명확한 옵션을 제시하고 선택한 워크플로우를 처리하여 개발 작업의 완료를 안내합니다.
- [iOS Simulator](https://github.com/conorluddy/ios-simulator-skill) - Claude가 iOS 애플리케이션을 테스트하고 디버깅하기 위해 iOS Simulator와 상호 작용할 수 있도록 합니다. *제작: [@conorluddy](https://github.com/conorluddy)*
- [MCP Builder](./mcp-builder/) - Python 또는 TypeScript를 사용하여 외부 API 및 서비스를 LLM과 통합하기 위한 고품질 MCP(Model Context Protocol) 서버 생성을 안내합니다.
- [move-code-quality-skill](https://github.com/1NickPappas/move-code-quality-skill) - Move 2024 Edition 규정 준수 및 모범 사례에 대한 공식 Move Book Code Quality Checklist에 따라 Move 언어 패키지를 분석합니다.
- [Playwright Browser Automation](https://github.com/lackeyjb/playwright-skill) - 웹 애플리케이션 테스트 및 검증을 위한 모델 호출 Playwright 자동화. *제작: [@lackeyjb](https://github.com/lackeyjb)*
- [prompt-engineering](https://github.com/NeoLabHQ/context-engineering-kit/tree/master/plugins/customaize-agent/skills/prompt-engineering) - Anthropic 모범 사례 및 에이전트 설득 원칙을 포함하여 잘 알려진 프롬프트 엔지니어링 기술과 패턴을 가르칩니다.
- [pypict-claude-skill](https://github.com/omkamal/pypict-claude-skill) - 요구 사항 또는 코드에 대해 PICT(Pairwise Independent Combinatorial Testing)를 사용하여 포괄적인 테스트 케이스를 설계하고 쌍별 커버리지로 최적화된 테스트 스위트를 생성합니다.
- [Skill Creator](./skill-creator/) - 전문 지식, 워크플로우 및 도구 통합으로 기능을 확장하는 효과적인 Claude Skills를 생성하는 방법에 대한 지침을 제공합니다.
- [Skill Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) - 몇 분 안에 모든 문서 웹사이트를 Claude AI 스킬로 자동 변환합니다. *제작: [@yusufkaraaslan](https://github.com/yusufkaraaslan)*
- [software-architecture](https://github.com/NeoLabHQ/context-engineering-kit/tree/master/plugins/ddd/skills/software-architecture) - Clean Architecture, SOLID 원칙 및 포괄적인 소프트웨어 설계 모범 사례를 포함한 디자인 패턴을 구현합니다.
- [subagent-driven-development](https://github.com/NeoLabHQ/context-engineering-kit/tree/master/plugins/sadd/skills/subagent-driven-development) - 빠르고 제어된 개발을 위해 반복 간 코드 검토 체크포인트를 사용하여 개별 작업에 대한 독립적인 서브에이전트를 디스패치합니다.
- [test-driven-development](https://github.com/obra/superpowers/tree/main/skills/test-driven-development) - 구현 코드를 작성하기 전에 기능 또는 버그 수정을 구현할 때 사용합니다.
- [using-git-worktrees](https://github.com/obra/superpowers/blob/main/skills/using-git-worktrees/) - 스마트 디렉토리 선택 및 안전 검증을 통해 격리된 git worktree를 생성합니다.
- [Webapp Testing](./webapp-testing/) - 프론트엔드 기능 확인, UI 동작 디버깅 및 스크린샷 캡처를 위해 Playwright를 사용하여 로컬 웹 애플리케이션을 테스트합니다.

### 데이터 및 분석

- [CSV Data Summarizer](https://github.com/coffeefuelbump/csv-data-summarizer-claude-skill) - 사용자 프롬프트 없이 CSV 파일을 자동으로 분석하고 시각화와 함께 포괄적인 인사이트를 생성합니다. *제작: [@coffeefuelbump](https://github.com/coffeefuelbump)*
- [root-cause-tracing](https://github.com/obra/superpowers/tree/main/skills/root-cause-tracing) - 실행 깊숙한 곳에서 오류가 발생하고 원래 트리거를 찾기 위해 다시 추적해야 할 때 사용합니다.

### 비즈니스 및 마케팅

- [Brand Guidelines](./brand-guidelines/) - 일관된 시각적 아이덴티티와 전문적인 디자인 표준을 위해 Anthropic의 공식 브랜드 색상 및 타이포그래피를 아티팩트에 적용합니다.
- [Competitive Ads Extractor](./competitive-ads-extractor/) - 광고 라이브러리에서 경쟁업체의 광고를 추출하고 분석하여 공감을 얻는 메시징과 크리에이티브 접근 방식을 이해합니다.
- [Domain Name Brainstormer](./domain-name-brainstormer/) - 창의적인 도메인 이름 아이디어를 생성하고 .com, .io, .dev, .ai 확장자를 포함한 여러 TLD에서 가용성을 확인합니다.
- [Internal Comms](./internal-comms/) - 회사별 형식을 사용하여 3P 업데이트, 회사 뉴스레터, FAQ, 상태 보고서 및 프로젝트 업데이트를 포함한 내부 커뮤니케이션을 작성하는 데 도움을 줍니다.
- [Lead Research Assistant](./lead-research-assistant/) - 제품을 분석하고, 타겟 회사를 검색하고, 실행 가능한 아웃리치 전략을 제공하여 고품질 리드를 식별하고 검증합니다.

### 커뮤니케이션 및 글쓰기

- [article-extractor](https://github.com/michalparkola/tapestry-skills-for-claude-code/tree/main/article-extractor) - 웹 페이지에서 전체 기사 텍스트와 메타데이터를 추출합니다.
- [brainstorming](https://github.com/obra/superpowers/tree/main/skills/brainstorming) - 구조화된 질문과 대안 탐색을 통해 대략적인 아이디어를 완전한 디자인으로 변환합니다.
- [Content Research Writer](./content-research-writer/) - 연구 수행, 인용 추가, 훅 개선 및 섹션별 피드백 제공을 통해 고품질 콘텐츠 작성을 지원합니다.
- [family-history-research](https://github.com/emaynard/claude-family-history-research-skill) - 가족 역사 및 족보 연구 프로젝트 계획을 지원합니다.
- [Meeting Insights Analyzer](./meeting-insights-analyzer/) - 회의 녹취록을 분석하여 갈등 회피, 발언 비율, 필러 단어 및 리더십 스타일을 포함한 행동 패턴을 파악합니다.
- [NotebookLM Integration](https://github.com/PleasePrompto/notebooklm-skill) - Claude Code가 업로드된 문서만을 기반으로 소스 기반 답변을 위해 NotebookLM과 직접 채팅할 수 있도록 합니다. *제작: [@PleasePrompto](https://github.com/PleasePrompto)*

### 크리에이티브 및 미디어

- [Canvas Design](./canvas-design/) - 포스터, 디자인 및 정적 작품을 위한 디자인 철학과 미학 원리를 사용하여 PNG 및 PDF 문서에서 아름다운 비주얼 아트를 만듭니다.
- [Image Enhancer](./image-enhancer/) - 전문 프레젠테이션 및 문서화를 위해 해상도, 선명도 및 명확도를 향상시켜 이미지 및 스크린샷 품질을 개선합니다.
- [Slack GIF Creator](./slack-gif-creator/) - 크기 제약 조건에 대한 유효성 검사기와 구성 가능한 애니메이션 프리미티브를 사용하여 Slack에 최적화된 애니메이션 GIF를 만듭니다.
- [Theme Factory](./theme-factory/) - 10개의 사전 설정 테마로 슬라이드, 문서, 보고서 및 HTML 랜딩 페이지를 포함한 아티팩트에 전문 글꼴 및 색상 테마를 적용합니다.
- [Video Downloader](./video-downloader/) - 다양한 형식과 품질 옵션을 지원하여 오프라인 시청, 편집 또는 보관을 위해 YouTube 및 기타 플랫폼에서 비디오를 다운로드합니다.
- [youtube-transcript](https://github.com/michalparkola/tapestry-skills-for-claude-code/tree/main/youtube-transcript) - YouTube 비디오에서 자막을 가져오고 요약을 준비합니다.

### 생산성 및 조직

- [File Organizer](./file-organizer/) - 컨텍스트를 이해하고, 중복을 찾고, 더 나은 조직 구조를 제안하여 파일과 폴더를 지능적으로 정리합니다.
- [Invoice Organizer](./invoice-organizer/) - 파일을 읽고, 정보를 추출하고, 일관되게 이름을 변경하여 세금 준비를 위해 인보이스 및 영수증을 자동으로 정리합니다.
- [kaizen](https://github.com/NeoLabHQ/context-engineering-kit/tree/master/plugins/kaizen/skills/kaizen) - 일본의 카이젠 철학과 린 방법론을 기반으로 여러 분석 접근 방식을 사용하여 지속적인 개선 방법론을 적용합니다.
- [Raffle Winner Picker](./raffle-winner-picker/) - 암호학적으로 안전한 무작위성을 사용하여 경품 및 콘테스트를 위해 목록, 스프레드시트 또는 Google Sheets에서 우승자를 무작위로 선택합니다.
- [ship-learn-next](https://github.com/michalparkola/tapestry-skills-for-claude-code/tree/main/ship-learn-next) - 피드백 루프를 기반으로 다음에 빌드하거나 학습할 항목을 반복하는 데 도움을 주는 스킬입니다.
- [tapestry](https://github.com/michalparkola/tapestry-skills-for-claude-code/tree/main/tapestry) - 관련 문서를 상호 연결하고 지식 네트워크로 요약합니다.

### 협업 및 프로젝트 관리

- [git-pushing](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/engineering-workflow-plugin/skills/git-pushing) - git 작업 및 저장소 상호 작용을 자동화합니다.
- [review-implementing](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/engineering-workflow-plugin/skills/review-implementing) - 코드 구현 계획을 평가하고 사양과 정렬합니다.
- [test-fixing](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/engineering-workflow-plugin/skills/test-fixing) - 실패한 테스트를 감지하고 패치 또는 수정을 제안합니다.

### 보안 및 시스템

- [computer-forensics](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/computer-forensics-skills/skills/computer-forensics) - 디지털 포렌식 분석 및 조사 기술.
- [file-deletion](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/computer-forensics-skills/skills/file-deletion) - 안전한 파일 삭제 및 데이터 삭제 방법.
- [metadata-extraction](https://github.com/mhattingpete/claude-skills-marketplace/tree/main/computer-forensics-skills/skills/metadata-extraction) - 포렌식 목적으로 파일 메타데이터를 추출하고 분석합니다.
- [threat-hunting-with-sigma-rules](https://github.com/jthack/threat-hunting-with-sigma-rules-skill) - Sigma 탐지 규칙을 사용하여 위협을 찾고 보안 이벤트를 분석합니다.

## 시작하기

### Claude.ai에서 스킬 사용하기

1. 채팅 인터페이스에서 스킬 아이콘(🧩)을 클릭합니다.
2. 마켓플레이스에서 스킬을 추가하거나 커스텀 스킬을 업로드합니다.
3. Claude가 작업에 따라 관련 스킬을 자동으로 활성화합니다.

### Claude Code에서 스킬 사용하기

1. `~/.config/claude-code/skills/`에 스킬을 배치합니다:
   ```bash
   mkdir -p ~/.config/claude-code/skills/
   cp -r skill-name ~/.config/claude-code/skills/
   ```

2. 스킬 메타데이터를 확인합니다:
   ```bash
   head ~/.config/claude-code/skills/skill-name/SKILL.md
   ```

3. Claude Code를 시작합니다:
   ```bash
   claude
   ```

4. 스킬이 자동으로 로드되고 관련될 때 활성화됩니다.

### API를 통해 스킬 사용하기

Claude Skills API를 사용하여 프로그래밍 방식으로 스킬을 로드하고 관리합니다:

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    skills=["skill-id-here"],
    messages=[{"role": "user", "content": "Your prompt"}]
)
```

자세한 내용은 [Skills API 문서](https://docs.claude.com/en/api/skills-guide)를 참조하세요.

## 스킬 만들기

### 스킬 구조

각 스킬은 YAML 프론트매터가 있는 `SKILL.md` 파일을 포함하는 폴더입니다:

```
skill-name/
├── SKILL.md          # 필수: 스킬 지침 및 메타데이터
├── scripts/          # 선택: 헬퍼 스크립트
├── templates/        # 선택: 문서 템플릿
└── resources/        # 선택: 참조 파일
```

### 기본 스킬 템플릿

```markdown
---
name: my-skill-name
description: 이 스킬이 무엇을 하고 언제 사용하는지에 대한 명확한 설명.
---

# My Skill Name

스킬의 목적과 기능에 대한 자세한 설명.

## 언제 이 스킬을 사용하는가

- 사용 사례 1
- 사용 사례 2
- 사용 사례 3

## 지침

[Claude가 이 스킬을 실행하는 방법에 대한 자세한 지침]

## 예제

[스킬이 실제로 작동하는 모습을 보여주는 실제 예제]
```

### 스킬 모범 사례

- 특정하고 반복 가능한 작업에 집중
- 명확한 예제 및 엣지 케이스 포함
- 최종 사용자가 아닌 Claude를 위한 지침 작성
- Claude.ai, Claude Code 및 API에서 테스트
- 전제 조건 및 종속성 문서화
- 오류 처리 지침 포함

## 기여하기

기여를 환영합니다! 다음 사항에 대한 자세한 내용은 [기여 가이드라인](CONTRIBUTING.md)을 참조하세요:

- 새 스킬을 제출하는 방법
- 스킬 품질 표준
- Pull request 프로세스
- 행동 강령

### 빠른 기여 단계

1. 스킬이 실제 사용 사례를 기반으로 하는지 확인
2. 기존 스킬에서 중복 확인
3. 스킬 구조 템플릿 따르기
4. 플랫폼 전체에서 스킬 테스트
5. 명확한 문서와 함께 Pull request 제출

## 리소스

### 공식 문서

- [Claude Skills 개요](https://www.anthropic.com/news/skills) - 공식 발표 및 기능
- [Skills 사용자 가이드](https://support.claude.com/en/articles/12512180-using-skills-in-claude) - Claude에서 스킬 사용 방법
- [커스텀 스킬 만들기](https://support.claude.com/en/articles/12512198-creating-custom-skills) - 스킬 개발 가이드
- [Skills API 문서](https://docs.claude.com/en/api/skills-guide) - API 통합 가이드
- [Agent Skills 블로그 게시물](https://anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) - 엔지니어링 심층 분석

### 커뮤니티 리소스

- [Anthropic Skills Repository](https://github.com/anthropics/skills) - 공식 예제 스킬
- [Claude Community](https://community.anthropic.com) - 다른 사용자와 스킬 논의
- [Skills Marketplace](https://claude.ai/marketplace) - 스킬 검색 및 공유

### 영감 및 사용 사례

- [Lenny's Newsletter](https://www.lennysnewsletter.com/p/everyone-should-be-using-claude-code) - 사람들이 Claude Code를 사용하는 50가지 방법
- [Notion Skills](https://www.notion.so/notiondevs/Notion-Skills-for-Claude-28da4445d27180c7af1df7d8615723d0) - Notion 통합 스킬


## 커뮤니티 참여하기

- Composio를 인증 설정과 통합하는 것에 대해 질문이 있으신가요? [빠른 통화 예약하기](https://calendly.com/thomas-composio/composio-enterprise-setup)
- [Twitter](https://x.com/composio)에서 팔로우하기
- [Discord 참여하기](https://discord.com/invite/composio)

## 라이선스

이 저장소는 Apache License 2.0에 따라 라이선스가 부여됩니다.

개별 스킬은 다른 라이선스를 가질 수 있습니다. 특정 라이선스 정보는 각 스킬의 폴더를 확인하세요.

---

**참고**: Claude Skills는 Claude.ai, Claude Code 및 Claude API에서 작동합니다. 스킬을 만들면 모든 플랫폼에서 이식 가능하므로 Claude를 사용하는 모든 곳에서 워크플로우가 일관되게 유지됩니다.
