---
name: mcp-builder
description: LLM이 잘 설계된 도구를 통해 외부 서비스와 상호 작용할 수 있도록 하는 고품질 MCP(Model Context Protocol) 서버를 만들기 위한 가이드. Python(FastMCP) 또는 Node/TypeScript(MCP SDK)에서 외부 API나 서비스를 통합하기 위해 MCP 서버를 구축할 때 사용합니다.
license: LICENSE.txt에 전체 조건이 명시되어 있습니다
---

# MCP 서버 개발 가이드

## 개요

LLM이 외부 서비스와 효과적으로 상호 작용할 수 있도록 하는 고품질 MCP(Model Context Protocol) 서버를 만들려면 이 스킬을 사용하십시오. MCP 서버는 LLM이 외부 서비스 및 API에 액세스할 수 있도록 하는 도구를 제공합니다. MCP 서버의 품질은 제공된 도구를 사용하여 LLM이 실제 작업을 얼마나 잘 수행할 수 있는지로 측정됩니다.

---

# 프로세스

## 🚀 고수준 워크플로우

고품질 MCP 서버를 만드는 것은 네 가지 주요 단계를 포함합니다:

### 단계 1: 심층 조사 및 계획

#### 1.1 에이전트 중심 디자인 원칙 이해

구현에 착수하기 전에 다음 원칙을 검토하여 AI 에이전트를 위한 도구를 설계하는 방법을 이해하십시오:

**API 엔드포인트만이 아닌 워크플로우를 위해 구축:**
- 기존 API 엔드포인트를 단순히 래핑하지 마십시오 - 사려 깊고 영향력이 큰 워크플로우 도구를 구축하십시오
- 관련 작업 통합(예: 가용성을 확인하고 이벤트를 생성하는 `schedule_event`)
- 개별 API 호출이 아닌 완전한 작업을 가능하게 하는 도구에 집중
- 에이전트가 실제로 수행해야 하는 워크플로우 고려

**제한된 컨텍스트를 위한 최적화:**
- 에이전트는 제한된 컨텍스트 윈도우를 가지고 있습니다 - 모든 토큰을 중요하게 만드십시오
- 포괄적인 데이터 덤프가 아닌 높은 신호 정보를 반환하십시오
- "간결한" 대 "상세한" 응답 형식 옵션 제공
- 기술 코드보다 사람이 읽을 수 있는 식별자를 기본으로(ID보다 이름)
- 에이전트의 컨텍스트 예산을 희소 자원으로 고려

**실행 가능한 오류 메시지 디자인:**
- 오류 메시지는 에이전트를 올바른 사용 패턴으로 안내해야 합니다
- 구체적인 다음 단계 제안: "결과를 줄이려면 filter='active_only' 사용 시도"
- 오류를 단순히 진단적이 아닌 교육적으로 만드십시오
- 명확한 피드백을 통해 에이전트가 적절한 도구 사용을 배우도록 도우십시오

**자연스러운 작업 세분화 따르기:**
- 도구 이름은 인간이 작업에 대해 생각하는 방식을 반영해야 합니다
- 발견 가능성을 위해 일관된 접두사로 관련 도구 그룹화
- API 구조만이 아닌 자연스러운 워크플로우를 중심으로 도구 설계

**평가 기반 개발 사용:**
- 초기에 현실적인 평가 시나리오 생성
- 에이전트 피드백이 도구 개선을 주도하도록 함
- 실제 에이전트 성능을 기반으로 빠르게 프로토타입하고 반복

#### 1.3 MCP 프로토콜 문서 연구

**최신 MCP 프로토콜 문서 가져오기:**

WebFetch를 사용하여 로드: `https://modelcontextprotocol.io/llms-full.txt`

이 포괄적인 문서에는 완전한 MCP 사양과 가이드라인이 포함되어 있습니다.

#### 1.4 프레임워크 문서 연구

**다음 참조 파일을 로드하고 읽으십시오:**

- **MCP 모범 사례**: [📋 모범 사례 보기](./reference/mcp_best_practices.md) - 모든 MCP 서버를 위한 핵심 가이드라인

**Python 구현의 경우 다음도 로드:**
- **Python SDK 문서**: WebFetch를 사용하여 `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md` 로드
- [🐍 Python 구현 가이드](./reference/python_mcp_server.md) - Python 관련 모범 사례 및 예제

**Node/TypeScript 구현의 경우 다음도 로드:**
- **TypeScript SDK 문서**: WebFetch를 사용하여 `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md` 로드
- [⚡ TypeScript 구현 가이드](./reference/node_mcp_server.md) - Node/TypeScript 관련 모범 사례 및 예제

#### 1.5 API 문서를 철저히 연구

서비스를 통합하려면 **모든** 사용 가능한 API 문서를 읽으십시오:
- 공식 API 참조 문서
- 인증 및 권한 부여 요구 사항
- 속도 제한 및 페이지네이션 패턴
- 오류 응답 및 상태 코드
- 사용 가능한 엔드포인트 및 매개변수
- 데이터 모델 및 스키마

**포괄적인 정보를 수집하려면 필요에 따라 웹 검색 및 WebFetch 도구를 사용하십시오.**

#### 1.6 포괄적인 구현 계획 생성

조사를 기반으로 다음을 포함하는 자세한 계획을 만드십시오:

**도구 선택:**
- 구현할 가장 가치 있는 엔드포인트/작업 나열
- 가장 일반적이고 중요한 사용 사례를 가능하게 하는 도구 우선순위 지정
- 복잡한 워크플로우를 가능하게 하기 위해 함께 작동하는 도구 고려

**공유 유틸리티 및 도우미:**
- 일반적인 API 요청 패턴 식별
- 페이지네이션 도우미 계획
- 필터링 및 서식 지정 유틸리티 설계
- 오류 처리 전략 계획

**입력/출력 디자인:**
- 입력 검증 모델 정의(Python의 경우 Pydantic, TypeScript의 경우 Zod)
- 일관된 응답 형식 설계(예: JSON 또는 Markdown) 및 구성 가능한 세부 수준(예: Detailed 또는 Concise)
- 대규모 사용 계획(수천 명의 사용자/리소스)
- 문자 제한 및 절단 전략 구현(예: 25,000 토큰)

**오류 처리 전략:**
- 우아한 실패 모드 계획
- 명확하고 실행 가능하며 LLM 친화적인 자연어 오류 메시지 설계로 추가 작업 촉구
- 속도 제한 및 시간 초과 시나리오 고려
- 인증 및 권한 부여 오류 처리

---

### 단계 2: 구현

포괄적인 계획을 가지고 있으므로 언어별 모범 사례를 따라 구현을 시작합니다.

#### 2.1 프로젝트 구조 설정

**Python의 경우:**
- 단일 `.py` 파일을 만들거나 복잡한 경우 모듈로 구성([🐍 Python 가이드](./reference/python_mcp_server.md) 참조)
- 도구 등록을 위해 MCP Python SDK 사용
- 입력 검증을 위해 Pydantic 모델 정의

**Node/TypeScript의 경우:**
- 적절한 프로젝트 구조 생성([⚡ TypeScript 가이드](./reference/node_mcp_server.md) 참조)
- `package.json` 및 `tsconfig.json` 설정
- MCP TypeScript SDK 사용
- 입력 검증을 위해 Zod 스키마 정의

#### 2.2 먼저 핵심 인프라 구현

**구현을 시작하려면 도구를 구현하기 전에 공유 유틸리티를 만드십시오:**
- API 요청 도우미 함수
- 오류 처리 유틸리티
- 응답 서식 지정 함수(JSON 및 Markdown)
- 페이지네이션 도우미
- 인증/토큰 관리

#### 2.3 도구를 체계적으로 구현

계획의 각 도구에 대해:

**입력 스키마 정의:**
- 검증을 위해 Pydantic(Python) 또는 Zod(TypeScript) 사용
- 적절한 제약 조건 포함(min/max 길이, 정규식 패턴, min/max 값, 범위)
- 명확하고 설명적인 필드 설명 제공
- 필드 설명에 다양한 예제 포함

**포괄적인 문서 문자열/설명 작성:**
- 도구가 하는 일에 대한 한 줄 요약
- 목적 및 기능에 대한 자세한 설명
- 예제가 포함된 명시적 매개변수 유형
- 완전한 반환 유형 스키마
- 사용 예제(언제 사용, 언제 사용하지 않음)
- 오류 처리 문서, 특정 오류가 주어지면 진행 방법 개요

**도구 로직 구현:**
- 코드 중복을 피하기 위해 공유 유틸리티 사용
- 모든 I/O에 대해 async/await 패턴 따르기
- 적절한 오류 처리 구현
- 여러 응답 형식 지원(JSON 및 Markdown)
- 페이지네이션 매개변수 준수
- 문자 제한 확인 및 적절하게 절단

**도구 주석 추가:**
- `readOnlyHint`: true(읽기 전용 작업의 경우)
- `destructiveHint`: false(파괴적이지 않은 작업의 경우)
- `idempotentHint`: true(반복 호출이 동일한 효과를 갖는 경우)
- `openWorldHint`: true(외부 시스템과 상호 작용하는 경우)

#### 2.4 언어별 모범 사례 따르기

**이 시점에서 적절한 언어 가이드를 로드하십시오:**

**Python의 경우: [🐍 Python 구현 가이드](./reference/python_mcp_server.md)를 로드하고 다음을 확인:**
- 적절한 도구 등록과 함께 MCP Python SDK 사용
- `model_config`가 있는 Pydantic v2 모델
- 전체에 걸친 유형 힌트
- 모든 I/O 작업에 대한 Async/await
- 적절한 import 구성
- 모듈 수준 상수(CHARACTER_LIMIT, API_BASE_URL)

**Node/TypeScript의 경우: [⚡ TypeScript 구현 가이드](./reference/node_mcp_server.md)를 로드하고 다음을 확인:**
- `server.registerTool`을 적절하게 사용
- `.strict()`가 있는 Zod 스키마
- TypeScript strict 모드 활성화
- `any` 유형 없음 - 적절한 유형 사용
- 명시적 Promise<T> 반환 유형
- 빌드 프로세스 구성(`npm run build`)

---

### 단계 3: 검토 및 개선

초기 구현 후:

#### 3.1 코드 품질 검토

품질을 보장하려면 다음에 대해 코드를 검토하십시오:
- **DRY 원칙**: 도구 간 중복 코드 없음
- **구성 가능성**: 공유 로직이 함수로 추출됨
- **일관성**: 유사한 작업이 유사한 형식을 반환
- **오류 처리**: 모든 외부 호출에 오류 처리 있음
- **유형 안전성**: 전체 유형 적용 범위(Python 유형 힌트, TypeScript 유형)
- **문서화**: 모든 도구에 포괄적인 문서 문자열/설명 있음

#### 3.2 테스트 및 빌드

**중요:** MCP 서버는 stdio/stdin 또는 sse/http를 통해 요청을 기다리는 장기 실행 프로세스입니다. 주 프로세스에서 직접 실행(예: `python server.py` 또는 `node dist/index.js`)하면 프로세스가 무기한 중단됩니다.

**서버를 테스트하는 안전한 방법:**
- 평가 하네스 사용(단계 4 참조) - 권장 접근 방식
- 주 프로세스 외부에 유지하기 위해 tmux에서 서버 실행
- 테스트할 때 시간 초과 사용: `timeout 5s python server.py`

**Python의 경우:**
- Python 구문 확인: `python -m py_compile your_server.py`
- 파일을 검토하여 import가 올바르게 작동하는지 확인
- 수동으로 테스트하려면: tmux에서 서버 실행 후 주 프로세스에서 평가 하네스로 테스트
- 또는 평가 하네스를 직접 사용(stdio 전송을 위해 서버 관리)

**Node/TypeScript의 경우:**
- `npm run build`를 실행하고 오류 없이 완료되는지 확인
- dist/index.js가 생성되었는지 확인
- 수동으로 테스트하려면: tmux에서 서버 실행 후 주 프로세스에서 평가 하네스로 테스트
- 또는 평가 하네스를 직접 사용(stdio 전송을 위해 서버 관리)

#### 3.3 품질 체크리스트 사용

구현 품질을 확인하려면 언어별 가이드에서 적절한 체크리스트를 로드하십시오:
- Python: [🐍 Python 가이드](./reference/python_mcp_server.md)의 "품질 체크리스트" 참조
- Node/TypeScript: [⚡ TypeScript 가이드](./reference/node_mcp_server.md)의 "품질 체크리스트" 참조

---

### 단계 4: 평가 생성

MCP 서버를 구현한 후 효과를 테스트하기 위한 포괄적인 평가를 생성합니다.

**완전한 평가 가이드라인을 위해 [✅ 평가 가이드](./reference/evaluation.md)를 로드하십시오.**

#### 4.1 평가 목적 이해

평가는 LLM이 현실적이고 복잡한 질문에 답하기 위해 MCP 서버를 효과적으로 사용할 수 있는지 테스트합니다.

#### 4.2 10개 평가 질문 생성

효과적인 평가를 생성하려면 평가 가이드에 설명된 프로세스를 따르십시오:

1. **도구 검사**: 사용 가능한 도구를 나열하고 기능 이해
2. **콘텐츠 탐색**: 읽기 전용 작업을 사용하여 사용 가능한 데이터 탐색
3. **질문 생성**: 10개의 복잡하고 현실적인 질문 생성
4. **답변 확인**: 각 질문을 직접 풀어 답변 확인

#### 4.3 평가 요구 사항

각 질문은 다음과 같아야 합니다:
- **독립적**: 다른 질문에 의존하지 않음
- **읽기 전용**: 파괴적이지 않은 작업만 필요
- **복잡함**: 여러 도구 호출 및 심층 탐색 필요
- **현실적**: 인간이 관심을 가질 실제 사용 사례 기반
- **검증 가능**: 문자열 비교로 확인할 수 있는 단일하고 명확한 답변
- **안정적**: 시간이 지나도 답변이 변경되지 않음

#### 4.4 출력 형식

다음 구조의 XML 파일 생성:

```xml
<evaluation>
  <qa_pair>
    <question>동물 코드명으로 AI 모델 출시에 대한 토론을 찾으십시오. 한 모델은 ASL-X 형식을 사용하는 특정 안전 지정이 필요했습니다. 점박이 야생 고양이의 이름을 딴 모델에 대해 결정되고 있던 숫자 X는 무엇입니까?</question>
    <answer>3</answer>
  </qa_pair>
<!-- 더 많은 qa_pairs... -->
</evaluation>
```

---

# 참조 파일

## 📚 문서 라이브러리

개발 중 필요에 따라 이러한 리소스를 로드하십시오:

### 핵심 MCP 문서(먼저 로드)
- **MCP 프로토콜**: `https://modelcontextprotocol.io/llms-full.txt`에서 가져오기 - 완전한 MCP 사양
- [📋 MCP 모범 사례](./reference/mcp_best_practices.md) - 다음을 포함한 보편적 MCP 가이드라인:
  - 서버 및 도구 명명 규칙
  - 응답 형식 가이드라인(JSON 대 Markdown)
  - 페이지네이션 모범 사례
  - 문자 제한 및 절단 전략
  - 도구 개발 가이드라인
  - 보안 및 오류 처리 표준

### SDK 문서(단계 1/2 중 로드)
- **Python SDK**: `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`에서 가져오기
- **TypeScript SDK**: `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`에서 가져오기

### 언어별 구현 가이드(단계 2 중 로드)
- [🐍 Python 구현 가이드](./reference/python_mcp_server.md) - 다음을 포함한 완전한 Python/FastMCP 가이드:
  - 서버 초기화 패턴
  - Pydantic 모델 예제
  - `@mcp.tool`로 도구 등록
  - 완전한 작동 예제
  - 품질 체크리스트

- [⚡ TypeScript 구현 가이드](./reference/node_mcp_server.md) - 다음을 포함한 완전한 TypeScript 가이드:
  - 프로젝트 구조
  - Zod 스키마 패턴
  - `server.registerTool`로 도구 등록
  - 완전한 작동 예제
  - 품질 체크리스트

### 평가 가이드(단계 4 중 로드)
- [✅ 평가 가이드](./reference/evaluation.md) - 다음을 포함한 완전한 평가 생성 가이드:
  - 질문 생성 가이드라인
  - 답변 확인 전략
  - XML 형식 사양
  - 예시 질문 및 답변
  - 제공된 스크립트로 평가 실행
