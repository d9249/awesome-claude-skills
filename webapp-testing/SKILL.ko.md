---
name: webapp-testing
description: Playwright를 사용하여 로컬 웹 애플리케이션과 상호 작용하고 테스트하는 툴킷입니다. 프론트엔드 기능 확인, UI 동작 디버깅, 브라우저 스크린샷 캡처 및 브라우저 로그 보기를 지원합니다.
license: 전체 조건은 LICENSE.txt 참조
---

# 웹 애플리케이션 테스팅

로컬 웹 애플리케이션을 테스트하려면 네이티브 Python Playwright 스크립트를 작성하세요.

**사용 가능한 도우미 스크립트**:
- `scripts/with_server.py` - 서버 수명 주기 관리 (여러 서버 지원)

**항상 먼저 `--help`와 함께 스크립트를 실행하세요** 사용법을 확인하기 위해. 스크립트를 먼저 실행해보고 사용자 정의 솔루션이 절대적으로 필요하다는 것을 발견할 때까지 소스를 읽지 마세요. 이러한 스크립트는 매우 클 수 있으며 컨텍스트 창을 오염시킵니다. 이들은 컨텍스트 창에 수집되기보다는 블랙박스 스크립트로 직접 호출되도록 존재합니다.

## 의사 결정 트리: 접근 방식 선택

```
사용자 작업 → 정적 HTML인가요?
    ├─ 예 → 선택자를 식별하기 위해 HTML 파일 직접 읽기
    │         ├─ 성공 → 선택자를 사용하여 Playwright 스크립트 작성
    │         └─ 실패/불완전 → 동적으로 처리 (아래)
    │
    └─ 아니오 (동적 웹앱) → 서버가 이미 실행 중인가요?
        ├─ 아니오 → 실행: python scripts/with_server.py --help
        │        그런 다음 도우미 + 단순화된 Playwright 스크립트 작성 사용
        │
        └─ 예 → 정찰 후 액션:
            1. 탐색하고 networkidle 대기
            2. 스크린샷 찍기 또는 DOM 검사
            3. 렌더링된 상태에서 선택자 식별
            4. 발견된 선택자로 액션 실행
```

## 예시: with_server.py 사용

서버를 시작하려면 먼저 `--help`를 실행한 다음 도우미를 사용하세요:

**단일 서버:**
```bash
python scripts/with_server.py --server "npm run dev" --port 5173 -- python your_automation.py
```

**여러 서버 (예: 백엔드 + 프론트엔드):**
```bash
python scripts/with_server.py \
  --server "cd backend && python server.py" --port 3000 \
  --server "cd frontend && npm run dev" --port 5173 \
  -- python your_automation.py
```

자동화 스크립트를 만들려면 Playwright 로직만 포함하세요 (서버는 자동으로 관리됨):
```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True) # 항상 headless 모드로 chromium 실행
    page = browser.new_page()
    page.goto('http://localhost:5173') # 서버가 이미 실행 중이고 준비됨
    page.wait_for_load_state('networkidle') # 중요: JS 실행 대기
    # ... 자동화 로직
    browser.close()
```

## 정찰 후 액션 패턴

1. **렌더링된 DOM 검사**:
   ```python
   page.screenshot(path='/tmp/inspect.png', full_page=True)
   content = page.content()
   page.locator('button').all()
   ```

2. 검사 결과에서 **선택자 식별**

3. 발견된 선택자를 사용하여 **액션 실행**

## 일반적인 함정

❌ **하지 말 것** 동적 앱에서 `networkidle`을 대기하기 전에 DOM 검사
✅ **할 것** 검사 전에 `page.wait_for_load_state('networkidle')` 대기

## 모범 사례

- **번들 스크립트를 블랙박스로 사용** - 작업을 수행하려면 `scripts/`에서 사용 가능한 스크립트 중 하나가 도움이 될 수 있는지 고려하세요. 이러한 스크립트는 컨텍스트 창을 어지럽히지 않고 일반적이고 복잡한 워크플로를 안정적으로 처리합니다. 사용법을 보려면 `--help`를 사용한 다음 직접 호출하세요.
- 동기 스크립트에 `sync_playwright()` 사용
- 완료되면 항상 브라우저 닫기
- 설명이 포함된 선택자 사용: `text=`, `role=`, CSS 선택자 또는 ID
- 적절한 대기 추가: `page.wait_for_selector()` 또는 `page.wait_for_timeout()`

## 참조 파일

- **examples/** - 일반적인 패턴을 보여주는 예시:
  - `element_discovery.py` - 페이지에서 버튼, 링크 및 입력 발견
  - `static_html_automation.py` - 로컬 HTML에 file:// URL 사용
  - `console_logging.py` - 자동화 중 콘솔 로그 캡처
