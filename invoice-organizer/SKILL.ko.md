---
name: invoice-organizer
description: 지저분한 파일을 읽고, 주요 정보를 추출하고, 일관되게 이름을 변경하고, 논리적인 폴더로 정렬하여 세금 준비를 위해 송장과 영수증을 자동으로 정리합니다. 수 시간의 수동 부기 작업을 몇 분의 자동화된 정리로 바꿉니다.
---

# 송장 정리자

이 스킬은 혼란스러운 송장, 영수증 및 재무 문서 폴더를 수동 작업 없이 깔끔하고 세금 준비가 완료된 파일링 시스템으로 변환합니다.

## 이 스킬을 사용해야 할 때

- 세금 시즌을 준비하고 정리된 기록이 필요함
- 여러 공급업체에 걸쳐 비즈니스 비용 관리
- 지저분한 폴더나 이메일 다운로드에서 영수증 정리
- 지속적인 부기를 위한 자동화된 송장 파일링 설정
- 연도나 카테고리별로 재무 기록 보관
- 환급을 위한 비용 조정
- 회계사를 위한 문서 준비

## 이 스킬이 하는 일

1. **송장 콘텐츠 읽기**: PDF, 이미지 및 문서에서 정보 추출:
   - 공급업체/회사 이름
   - 송장 번호
   - 날짜
   - 금액
   - 제품 또는 서비스 설명
   - 결제 방법

2. **파일 이름을 일관되게 변경**: 표준화된 파일 이름 생성:
   - 형식: `YYYY-MM-DD Vendor - Invoice - ProductOrService.pdf`
   - 예: `2024-03-15 Adobe - Invoice - Creative Cloud.pdf`

3. **카테고리별로 정리**: 논리적인 폴더로 정렬:
   - 공급업체별
   - 비용 카테고리별(소프트웨어, 사무실, 여행 등)
   - 시간 기간별(연도, 분기, 월)
   - 세금 카테고리별(공제 가능, 개인 등)

4. **여러 형식 처리**: 다음과 작동:
   - PDF 송장
   - 스캔된 영수증(JPG, PNG)
   - 이메일 첨부 파일
   - 스크린샷
   - 은행 명세서

5. **원본 유지**: 사본을 정리하면서 원본 파일 보존

## 사용 방법

### 기본 사용법

지저분한 송장 폴더로 이동:
```
cd ~/Desktop/receipts-to-sort
```

그런 다음 Claude Code에 요청:
```
이 송장을 세금을 위해 정리해주세요
```

또는 더 구체적으로:
```
이 폴더의 모든 송장을 읽고,
"YYYY-MM-DD Vendor - Invoice - Product.pdf" 형식으로 이름을 변경하고,
공급업체별로 정리해주세요
```

### 고급 정리

```
이 송장을 정리해주세요:
1. 각 파일에서 날짜, 공급업체 및 설명 추출
2. 표준 형식으로 이름 변경
3. 비용 카테고리별로 폴더에 정렬(소프트웨어, 사무실, 여행 등)
4. 회계사를 위해 모든 송장 세부 정보가 포함된 CSV 스프레드시트 생성
```

## 지침

사용자가 송장 정리를 요청할 때:

1. **폴더 스캔**

   모든 송장 파일 식별:
   ```bash
   # 모든 송장 관련 파일 찾기
   find . -type f \( -name "*.pdf" -o -name "*.jpg" -o -name "*.png" \) -print
   ```

   발견 사항 보고:
   - 총 파일 수
   - 파일 유형
   - 날짜 범위(이름에서 식별 가능한 경우)
   - 현재 정리 상태(또는 부족)

2. **각 파일에서 정보 추출**

   각 송장에 대해 추출:

   **PDF 송장에서**:
   - 텍스트 추출을 사용하여 송장 콘텐츠 읽기
   - 일반적인 패턴 찾기:
     - "Invoice Date:", "Date:", "Issued:"
     - "Invoice #:", "Invoice Number:"
     - 회사 이름(일반적으로 상단)
     - "Amount Due:", "Total:", "Amount:"
     - "Description:", "Service:", "Product:"

   **이미지 영수증에서**:
   - 이미지에서 보이는 텍스트 읽기
   - 공급업체 이름 식별(종종 상단)
   - 날짜 찾기(일반적인 형식)
   - 총 금액 찾기

   **불명확한 파일의 대체**:
   - 파일 이름 단서 사용
   - 파일 생성/수정 날짜 확인
   - 중요한 정보가 누락된 경우 수동 검토 플래그

3. **정리 전략 결정**

   지정되지 않은 경우 사용자 선호도 질문:

   ```markdown
   [날짜 범위]의 [X]개 송장을 찾았습니다.

   어떻게 정리하시겠습니까?

   1. **공급업체별**(Adobe/, Amazon/, Stripe/ 등)
   2. **카테고리별**(Software/, Office Supplies/, Travel/ 등)
   3. **날짜별**(2024/Q1/, 2024/Q2/ 등)
   4. **세금 카테고리별**(Deductible/, Personal/ 등)
   5. **사용자 정의**(구조 설명)

   또는 기본 구조를 사용할 수 있습니다: Year/Category/Vendor
   ```

4. **표준화된 파일 이름 생성**

   각 송장에 대해 다음 패턴을 따르는 파일 이름 생성:

   ```
   YYYY-MM-DD Vendor - Invoice - Description.ext
   ```

   예:
   - `2024-03-15 Adobe - Invoice - Creative Cloud.pdf`
   - `2024-01-10 Amazon - Receipt - Office Supplies.pdf`
   - `2023-12-01 Stripe - Invoice - Monthly Payment Processing.pdf`

   **파일 이름 모범 사례**:
   - 하이픈을 제외한 특수 문자 제거
   - 공급업체 이름을 적절하게 대문자화
   - 설명을 간결하지만 의미 있게 유지
   - 정렬을 위해 일관된 날짜 형식(YYYY-MM-DD) 사용
   - 원래 파일 확장자 보존

5. **정리 실행**

   파일을 이동하기 전에 계획 표시:

   ```markdown
   # 정리 계획

   ## 제안된 구조
   ```
   Invoices/
   ├── 2023/
   │   ├── Software/
   │   │   ├── Adobe/
   │   │   └── Microsoft/
   │   ├── Services/
   │   └── Office/
   └── 2024/
       ├── Software/
       ├── Services/
       └── Office/
   ```

   ## 샘플 변경 사항

   변경 전: `invoice_adobe_march.pdf`
   변경 후: `2024-03-15 Adobe - Invoice - Creative Cloud.pdf`
   위치: `Invoices/2024/Software/Adobe/`

   변경 전: `IMG_2847.jpg`
   변경 후: `2024-02-10 Staples - Receipt - Office Supplies.jpg`
   위치: `Invoices/2024/Office/Staples/`

   [X]개 파일을 처리할까요? (예/아니오)
   ```

   승인 후:
   ```bash
   # 폴더 구조 만들기
   mkdir -p "Invoices/2024/Software/Adobe"

   # 원본을 보존하기 위해 복사(이동하지 않음)
   cp "original.pdf" "Invoices/2024/Software/Adobe/2024-03-15 Adobe - Invoice - Creative Cloud.pdf"

   # 또는 사용자가 선호하는 경우 이동
   mv "original.pdf" "new/path/standardized-name.pdf"
   ```

6. **요약 보고서 생성**

   모든 송장 세부 정보가 포함된 CSV 파일 생성:

   ```csv
   Date,Vendor,Invoice Number,Description,Amount,Category,File Path
   2024-03-15,Adobe,INV-12345,Creative Cloud,52.99,Software,Invoices/2024/Software/Adobe/2024-03-15 Adobe - Invoice - Creative Cloud.pdf
   2024-03-10,Amazon,123-4567890-1234567,Office Supplies,127.45,Office,Invoices/2024/Office/Amazon/2024-03-10 Amazon - Receipt - Office Supplies.pdf
   ...
   ```

   이 CSV는 다음에 유용합니다:
   - 회계 소프트웨어로 가져오기
   - 회계사와 공유
   - 비용 추적 및 보고
   - 세금 준비

7. **완료 요약 제공**

   ```markdown
   # 정리 완료! 📊

   ## 요약
   - **처리됨**: [X]개 송장
   - **날짜 범위**: [가장 빠른] ~ [가장 늦은]
   - **총 금액**: $[합계](금액이 추출된 경우)
   - **공급업체**: [Y]개 고유 공급업체

   ## 새로운 구조
   ```
   Invoices/
   ├── 2024/ (45개 파일)
   │   ├── Software/ (23개 파일)
   │   ├── Services/ (12개 파일)
   │   └── Office/ (10개 파일)
   └── 2023/ (12개 파일)
   ```

   ## 생성된 파일
   - `/Invoices/` - 정리된 송장
   - `/Invoices/invoice-summary.csv` - 회계용 스프레드시트
   - `/Invoices/originals/` - 원본 파일(복사된 경우)

   ## 검토가 필요한 파일
   [정보를 완전히 추출할 수 없는 파일 나열]

   ## 다음 단계
   1. `invoice-summary.csv` 파일 검토
   2. "Needs Review" 폴더의 파일 확인
   3. CSV를 회계 소프트웨어로 가져오기
   4. 향후 송장을 위한 자동 정리 설정

   세금 시즌 준비 완료! 🎉
   ```

## 예시

### 예시 1: 세금 준비(Martin Merschroth에서)

**사용자**: "세금을 위한 지저분한 송장 폴더가 있습니다. 정렬하고 적절하게 이름을 변경해주세요."

**프로세스**:
1. 폴더 스캔: 147개 PDF 및 이미지 발견
2. 각 송장을 읽어 추출:
   - 날짜
   - 공급업체 이름
   - 송장 번호
   - 제품/서비스 설명
3. 모든 파일 이름 변경: `YYYY-MM-DD Vendor - Invoice - Product.pdf`
4. 정리: `2024/Software/`, `2024/Travel/` 등
5. 회계사를 위한 `invoice-summary.csv` 생성
6. 결과: 몇 분 안에 세금 준비가 완료된 정리된 송장

### 예시 2: 월간 비용 조정

**사용자**: "지난달 비즈니스 영수증을 카테고리별로 정리해주세요."

**출력**:
```markdown
# 2024년 3월 영수증 정리됨

## 카테고리별
- 소프트웨어 & 도구: $847.32 (12개 송장)
- 사무용품: $234.18 (8개 영수증)
- 여행 & 식사: $1,456.90 (15개 영수증)
- 전문 서비스: $2,500.00 (3개 송장)

총: $5,038.40

모든 영수증이 다음에 이름이 변경되고 파일링됨:
`Business-Receipts/2024/03-March/[Category]/`

CSV 내보내기: `march-2024-expenses.csv`
```

### 예시 3: 다년간 아카이브

**사용자**: "3년치 무작위 송장이 있습니다. 연도별로 정리한 다음 공급업체별로 정리해주세요."

**출력**: 구조 생성:
```
Invoices/
├── 2022/
│   ├── Adobe/
│   ├── Amazon/
│   └── ...
├── 2023/
│   ├── Adobe/
│   ├── Amazon/
│   └── ...
└── 2024/
    ├── Adobe/
    ├── Amazon/
    └── ...
```

각 파일은 날짜와 설명으로 적절하게 이름이 변경됩니다.

### 예시 4: 이메일 다운로드 정리

**사용자**: "Gmail에서 송장을 다운로드합니다. 모두 'invoice.pdf', 'invoice(1).pdf' 등으로 이름이 지정되어 있습니다. 이 혼란을 해결해주세요."

**출력**:
```markdown
모두 "invoice*.pdf"로 이름이 지정된 89개 파일 발견

실제 정보를 추출하기 위해 각 파일 읽는 중...

이름 변경 예:
- invoice.pdf → 2024-03-15 Shopify - Invoice - Monthly Subscription.pdf
- invoice(1).pdf → 2024-03-14 Google - Invoice - Workspace.pdf
- invoice(2).pdf → 2024-03-10 Netlify - Invoice - Pro Plan.pdf

모든 파일이 이름이 변경되고 공급업체별로 정리됨.
```

## 일반적인 정리 패턴

### 공급업체별(간단함)
```
Invoices/
├── Adobe/
├── Amazon/
├── Google/
└── Microsoft/
```

### 연도 및 카테고리별(세금 친화적)
```
Invoices/
├── 2023/
│   ├── Software/
│   ├── Hardware/
│   ├── Services/
│   └── Travel/
└── 2024/
    └── ...
```

### 분기별(세부 추적)
```
Invoices/
├── 2024/
│   ├── Q1/
│   │   ├── Software/
│   │   ├── Office/
│   │   └── Travel/
│   └── Q2/
│       └── ...
```

### 세금 카테고리별(회계사 준비)
```
Invoices/
├── Deductible/
│   ├── Software/
│   ├── Office/
│   └── Professional-Services/
├── Partially-Deductible/
│   └── Meals-Travel/
└── Personal/
```

## 자동화 설정

지속적인 정리를 위해:

```
~/Downloads/invoices 폴더를 감시하고
표준 명명 및 폴더 구조를 사용하여 새 송장 파일을
자동으로 정리하는 스크립트를 만들어주세요.
```

이것은 도착하는 대로 송장을 정리하는 지속적인 솔루션을 만듭니다.

## 전문가 팁

1. **이메일을 PDF로 스캔**: Preview 또는 유사한 것을 사용하여 먼저 이메일 송장을 PDF로 저장
2. **일관된 다운로드**: 모든 송장을 일괄 처리를 위해 하나의 폴더에 저장
3. **월간 루틴**: 연간이 아닌 월간으로 송장 정리
4. **원본 백업**: 재정리하기 전에 원본 파일 유지
5. **CSV에 금액 포함**: 예산 추적에 유용
6. **공제 가능성별 태그**: 어떤 비용이 세금 공제 가능한지 메모
7. **영수증 7년 보관**: 표준 감사 기간

## 특수 사례 처리

### 누락된 정보
날짜/공급업체를 추출할 수 없는 경우:
- 수동 검토를 위해 파일 플래그
- 대체로 파일 수정 날짜 사용
- "Needs-Review/" 폴더 생성

### 중복 송장
동일한 송장이 여러 번 나타나는 경우:
- 파일 해시 비교
- 최고 품질 버전 유지
- 요약에서 중복 메모

### 여러 페이지 송장
파일로 분할된 송장의 경우:
- 필요한 경우 PDF 병합
- 부분에 일관된 명명 사용
- 송장이 분할된 경우 CSV에 메모

### 비표준 형식
특이한 영수증 형식의 경우:
- 가능한 것 추출
- 가능한 것 표준화
- 중요한 정보가 누락된 경우 검토 플래그

## 관련 사용 사례

- 환급을 위한 비용 보고서 작성
- 은행 명세서 정리
- 공급업체 계약 관리
- 오래된 재무 기록 보관
- 감사 준비
- 시간 경과에 따른 구독 비용 추적
