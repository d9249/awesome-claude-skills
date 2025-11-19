---
name: docx
description: "추적된 변경 사항, 댓글, 포맷 보존 및 텍스트 추출을 지원하는 포괄적인 문서 생성, 편집 및 분석. Claude가 전문 문서(.docx 파일)로 작업해야 할 때: (1) 새 문서 생성, (2) 콘텐츠 수정 또는 편집, (3) 추적된 변경 사항 작업, (4) 댓글 추가 또는 기타 문서 작업"
license: 독점. LICENSE.txt에 완전한 약관이 있습니다
---

# DOCX 생성, 편집 및 분석

## 개요

사용자가 .docx 파일의 내용을 생성, 편집 또는 분석하도록 요청할 수 있습니다. .docx 파일은 본질적으로 XML 파일 및 기타 리소스를 포함하는 ZIP 아카이브로, 읽거나 편집할 수 있습니다. 다양한 작업에 사용할 수 있는 다양한 도구와 워크플로우가 있습니다.

## 워크플로우 결정 트리

### 콘텐츠 읽기/분석
아래 "텍스트 추출" 또는 "원시 XML 액세스" 섹션 사용

### 새 문서 생성
"새 Word 문서 생성" 워크플로우 사용

### 기존 문서 편집
- **자신의 문서 + 간단한 변경**
  "기본 OOXML 편집" 워크플로우 사용

- **다른 사람의 문서**
  **"Redlining 워크플로우"** 사용 (권장 기본값)

- **법률, 학술, 비즈니스 또는 정부 문서**
  **"Redlining 워크플로우"** 사용 (필수)

## 콘텐츠 읽기 및 분석

### 텍스트 추출
문서의 텍스트 내용만 읽으려면 pandoc을 사용하여 문서를 마크다운으로 변환해야 합니다. Pandoc은 문서 구조를 보존하고 추적된 변경 사항을 표시하는 데 탁월한 지원을 제공합니다:

```bash
# 추적된 변경 사항이 있는 문서를 마크다운으로 변환
pandoc --track-changes=all path-to-file.docx -o output.md
# 옵션: --track-changes=accept/reject/all
```

### 원시 XML 액세스
다음을 위해서는 원시 XML 액세스가 필요합니다: 댓글, 복잡한 포맷팅, 문서 구조, 임베디드 미디어 및 메타데이터. 이러한 기능 중 하나에 대해서는 문서의 압축을 풀고 원시 XML 콘텐츠를 읽어야 합니다.

#### 파일 압축 풀기
`python ooxml/scripts/unpack.py <office_file> <output_directory>`

#### 주요 파일 구조
* `word/document.xml` - 주요 문서 내용
* `word/comments.xml` - document.xml에서 참조되는 댓글
* `word/media/` - 임베디드 이미지 및 미디어 파일
* 추적된 변경 사항은 `<w:ins>` (삽입) 및 `<w:del>` (삭제) 태그 사용

## 새 Word 문서 생성

처음부터 새 Word 문서를 만들 때는 JavaScript/TypeScript를 사용하여 Word 문서를 만들 수 있는 **docx-js**를 사용하세요.

### 워크플로우
1. **필수 - 전체 파일 읽기**: [`docx-js.md`](docx-js.md) (~500줄)를 처음부터 끝까지 완전히 읽으세요. **이 파일을 읽을 때 범위 제한을 절대 설정하지 마세요.** 문서 생성을 진행하기 전에 자세한 구문, 중요한 포맷팅 규칙 및 모범 사례를 위해 전체 파일 내용을 읽으세요.
2. Document, Paragraph, TextRun 컴포넌트를 사용하여 JavaScript/TypeScript 파일 생성 (모든 종속성이 설치되어 있다고 가정할 수 있지만, 그렇지 않은 경우 아래 종속성 섹션 참조)
3. Packer.toBuffer()를 사용하여 .docx로 내보내기

## 기존 Word 문서 편집

기존 Word 문서를 편집할 때는 OOXML 조작을 위한 Python 라이브러리인 **Document 라이브러리**를 사용하세요. 라이브러리는 인프라 설정을 자동으로 처리하고 문서 조작을 위한 메서드를 제공합니다. 복잡한 시나리오의 경우 라이브러리를 통해 기본 DOM에 직접 액세스할 수 있습니다.

### 워크플로우
1. **필수 - 전체 파일 읽기**: [`ooxml.md`](ooxml.md) (~600줄)를 처음부터 끝까지 완전히 읽으세요. **이 파일을 읽을 때 범위 제한을 절대 설정하지 마세요.** Document 라이브러리 API 및 문서 파일을 직접 편집하기 위한 XML 패턴을 위해 전체 파일 내용을 읽으세요.
2. 문서 압축 풀기: `python ooxml/scripts/unpack.py <office_file> <output_directory>`
3. Document 라이브러리를 사용하여 Python 스크립트 생성 및 실행 (ooxml.md의 "Document Library" 섹션 참조)
4. 최종 문서 압축: `python ooxml/scripts/pack.py <input_directory> <office_file>`

Document 라이브러리는 일반적인 작업을 위한 상위 수준 메서드와 복잡한 시나리오를 위한 직접 DOM 액세스를 모두 제공합니다.

## 문서 검토를 위한 Redlining 워크플로우

이 워크플로우를 사용하면 OOXML에서 구현하기 전에 마크다운을 사용하여 포괄적인 추적된 변경 사항을 계획할 수 있습니다. **핵심**: 완전한 추적된 변경을 위해서는 모든 변경 사항을 체계적으로 구현해야 합니다.

**일괄 처리 전략**: 관련 변경 사항을 3-10개 변경의 일괄 처리로 그룹화하세요. 이렇게 하면 효율성을 유지하면서 디버깅을 관리하기 쉽게 만듭니다. 다음으로 이동하기 전에 각 일괄 처리를 테스트하세요.

**원칙: 최소한의 정확한 편집**
추적된 변경 사항을 구현할 때 실제로 변경되는 텍스트만 표시하세요. 변경되지 않은 텍스트를 반복하면 편집을 검토하기 어렵게 만들고 전문적이지 않게 보입니다. 대체를 다음으로 분할하세요: [변경되지 않은 텍스트] + [삭제] + [삽입] + [변경되지 않은 텍스트]. `<w:r>` 요소를 원본에서 추출하고 재사용하여 변경되지 않은 텍스트에 대한 원본 실행의 RSID를 보존하세요.

예 - 문장에서 "30 days"를 "60 days"로 변경:
```python
# 나쁨 - 전체 문장 대체
'<w:del><w:r><w:delText>The term is 30 days.</w:delText></w:r></w:del><w:ins><w:r><w:t>The term is 60 days.</w:t></w:r></w:ins>'

# 좋음 - 변경된 것만 표시, 변경되지 않은 텍스트에 대한 원본 <w:r> 보존
'<w:r w:rsidR="00AB12CD"><w:t>The term is </w:t></w:r><w:del><w:r><w:delText>30</w:delText></w:r></w:del><w:ins><w:r><w:t>60</w:t></w:r></w:ins><w:r w:rsidR="00AB12CD"><w:t> days.</w:t></w:r>'
```

### 추적된 변경 워크플로우

1. **마크다운 표현 가져오기**: 추적된 변경 사항이 보존된 문서를 마크다운으로 변환:
   ```bash
   pandoc --track-changes=all path-to-file.docx -o current.md
   ```

2. **변경 사항 식별 및 그룹화**: 문서를 검토하고 필요한 모든 변경 사항을 식별하여 논리적 일괄 처리로 구성:

   **위치 방법** (XML에서 변경 사항 찾기):
   - 섹션/제목 번호 (예: "Section 3.2", "Article IV")
   - 번호가 매겨진 경우 단락 식별자
   - 고유한 주변 텍스트가 있는 Grep 패턴
   - 문서 구조 (예: "first paragraph", "signature block")
   - **마크다운 줄 번호를 사용하지 마세요** - XML 구조에 매핑되지 않습니다

   **일괄 처리 조직** (일괄 처리당 3-10개의 관련 변경 그룹화):
   - 섹션별: "Batch 1: Section 2 amendments", "Batch 2: Section 5 updates"
   - 유형별: "Batch 1: Date corrections", "Batch 2: Party name changes"
   - 복잡성별: 간단한 텍스트 대체로 시작한 다음 복잡한 구조 변경 처리
   - 순차적: "Batch 1: Pages 1-3", "Batch 2: Pages 4-6"

3. **문서 읽기 및 압축 풀기**:
   - **필수 - 전체 파일 읽기**: [`ooxml.md`](ooxml.md) (~600줄)를 처음부터 끝까지 완전히 읽으세요. **이 파일을 읽을 때 범위 제한을 절대 설정하지 마세요.** "Document Library" 및 "Tracked Change Patterns" 섹션에 특별히 주의를 기울이세요.
   - **문서 압축 풀기**: `python ooxml/scripts/unpack.py <file.docx> <dir>`
   - **제안된 RSID 메모**: 압축 풀기 스크립트는 추적된 변경에 사용할 RSID를 제안합니다. 4b 단계에서 사용하기 위해 이 RSID를 복사하세요.

4. **일괄 처리로 변경 구현**: 변경 사항을 논리적으로 그룹화하고 (섹션별, 유형별 또는 근접성별) 단일 스크립트로 함께 구현하세요. 이 접근 방식:
   - 디버깅을 더 쉽게 만듭니다 (더 작은 일괄 처리 = 오류를 격리하기 더 쉬움)
   - 점진적인 진행 허용
   - 효율성 유지 (3-10개 변경의 일괄 처리 크기가 잘 작동합니다)

   **제안된 일괄 처리 그룹화:**
   - 문서 섹션별 (예: "Section 3 changes", "Definitions", "Termination clause")
   - 변경 유형별 (예: "Date changes", "Party name updates", "Legal term replacements")
   - 근접성별 (예: "Changes on pages 1-3", "Changes in first half of document")

   관련 변경의 각 일괄 처리에 대해:

   **a. 텍스트를 XML에 매핑**: `word/document.xml`에서 텍스트를 grep하여 `<w:r>` 요소에 걸쳐 텍스트가 어떻게 분할되는지 확인합니다.

   **b. 스크립트 생성 및 실행**: `get_node`를 사용하여 노드를 찾고, 변경을 구현한 다음 `doc.save()`를 수행합니다. 패턴은 ooxml.md의 **"Document Library"** 섹션을 참조하세요.

   **참고**: 스크립트를 작성하기 직전에 항상 `word/document.xml`을 grep하여 현재 줄 번호를 가져오고 텍스트 내용을 확인하세요. 줄 번호는 각 스크립트 실행 후 변경됩니다.

5. **문서 압축**: 모든 일괄 처리가 완료된 후 압축 해제된 디렉토리를 .docx로 다시 변환:
   ```bash
   python ooxml/scripts/pack.py unpacked reviewed-document.docx
   ```

6. **최종 확인**: 완전한 문서의 포괄적인 확인 수행:
   - 최종 문서를 마크다운으로 변환:
     ```bash
     pandoc --track-changes=all reviewed-document.docx -o verification.md
     ```
   - 모든 변경 사항이 올바르게 적용되었는지 확인:
     ```bash
     grep "original phrase" verification.md  # 찾으면 안 됨
     grep "replacement phrase" verification.md  # 찾아야 함
     ```
   - 의도하지 않은 변경이 도입되지 않았는지 확인


## 문서를 이미지로 변환

Word 문서를 시각적으로 분석하려면 두 단계 프로세스를 사용하여 이미지로 변환하세요:

1. **DOCX를 PDF로 변환**:
   ```bash
   soffice --headless --convert-to pdf document.docx
   ```

2. **PDF 페이지를 JPEG 이미지로 변환**:
   ```bash
   pdftoppm -jpeg -r 150 document.pdf page
   ```
   이것은 `page-1.jpg`, `page-2.jpg` 등과 같은 파일을 생성합니다.

옵션:
- `-r 150`: 해상도를 150 DPI로 설정 (품질/크기 균형 조정)
- `-jpeg`: JPEG 형식으로 출력 (선호하는 경우 PNG는 `-png` 사용)
- `-f N`: 변환할 첫 페이지 (예: `-f 2`는 2페이지부터 시작)
- `-l N`: 변환할 마지막 페이지 (예: `-l 5`는 5페이지에서 중지)
- `page`: 출력 파일의 접두사

특정 범위 예:
```bash
pdftoppm -jpeg -r 150 -f 2 -l 5 document.pdf page  # 2-5페이지만 변환
```

## 코드 스타일 가이드라인
**중요**: DOCX 작업을 위한 코드를 생성할 때:
- 간결한 코드 작성
- 장황한 변수 이름과 중복 작업 피하기
- 불필요한 print 문 피하기

## 종속성

필요한 종속성 (사용 가능하지 않은 경우 설치):

- **pandoc**: `sudo apt-get install pandoc` (텍스트 추출용)
- **docx**: `npm install -g docx` (새 문서 생성용)
- **LibreOffice**: `sudo apt-get install libreoffice` (PDF 변환용)
- **Poppler**: `sudo apt-get install poppler-utils` (pdftoppm으로 PDF를 이미지로 변환)
- **defusedxml**: `pip install defusedxml` (안전한 XML 파싱용)
