---
name: xlsx
description: "수식, 서식 지정, 데이터 분석 및 시각화를 지원하는 포괄적인 스프레드시트 생성, 편집 및 분석. Claude가 다음과 같은 경우 스프레드시트(.xlsx, .xlsm, .csv, .tsv 등) 작업이 필요할 때: (1) 수식과 서식이 있는 새 스프레드시트 생성, (2) 데이터 읽기 또는 분석, (3) 수식을 보존하면서 기존 스프레드시트 수정, (4) 스프레드시트에서 데이터 분석 및 시각화, 또는 (5) 수식 재계산"
license: Proprietary. LICENSE.txt에 전체 조건이 명시되어 있습니다
---

# 출력 요구 사항

## 모든 Excel 파일

### 수식 오류 제로
- 모든 Excel 모델은 수식 오류 제로(#REF!, #DIV/0!, #VALUE!, #N/A, #NAME?)로 제공되어야 합니다

### 기존 템플릿 보존(템플릿 업데이트 시)
- 파일을 수정할 때 기존 형식, 스타일 및 규칙을 연구하고 정확히 일치시킵니다
- 확립된 패턴이 있는 파일에 표준화된 서식을 강요하지 마십시오
- 기존 템플릿 규칙이 항상 이러한 지침보다 우선합니다

## 재무 모델

### 색상 코딩 표준
사용자 또는 기존 템플릿에서 달리 명시하지 않는 한

#### 업계 표준 색상 규칙
- **파란색 텍스트(RGB: 0,0,255)**: 하드코딩된 입력 및 사용자가 시나리오에 대해 변경할 숫자
- **검은색 텍스트(RGB: 0,0,0)**: 모든 수식 및 계산
- **녹색 텍스트(RGB: 0,128,0)**: 동일한 통합 문서 내의 다른 워크시트에서 가져오는 링크
- **빨간색 텍스트(RGB: 255,0,0)**: 다른 파일에 대한 외부 링크
- **노란색 배경(RGB: 255,255,0)**: 주의가 필요한 주요 가정 또는 업데이트해야 하는 셀

### 숫자 서식 표준

#### 필수 서식 규칙
- **연도**: 텍스트 문자열로 서식 지정(예: "2,024"가 아닌 "2024")
- **통화**: $#,##0 형식 사용; 헤더에 항상 단위 지정("수익 ($mm)")
- **0**: 숫자 서식을 사용하여 모든 0을 "-"로 만들고, 백분율 포함(예: "$#,##0;($#,##0);-")
- **백분율**: 기본적으로 0.0% 형식(소수점 한 자리)
- **배수**: 평가 배수(EV/EBITDA, P/E)에 대해 0.0x로 서식 지정
- **음수**: 마이너스 -123이 아닌 괄호(123) 사용

### 수식 구성 규칙

#### 가정 배치
- 모든 가정(성장률, 마진, 배수 등)을 별도의 가정 셀에 배치합니다
- 수식에서 하드코딩된 값 대신 셀 참조를 사용합니다
- 예: =B5*1.05 대신 =B5*(1+$B$6) 사용

#### 수식 오류 방지
- 모든 셀 참조가 올바른지 확인합니다
- 범위에서 off-by-one 오류를 확인합니다
- 모든 예측 기간에서 일관된 수식을 보장합니다
- 엣지 케이스(0 값, 음수)로 테스트합니다
- 의도하지 않은 순환 참조가 없는지 확인합니다

#### 하드코드에 대한 문서화 요구 사항
- 주석 또는 셀 옆(테이블 끝인 경우)에 추가. 형식: "출처: [시스템/문서], [날짜], [특정 참조], [해당되는 경우 URL]"
- 예:
  - "출처: 회사 10-K, FY2024, 45페이지, 수익 노트, [SEC EDGAR URL]"
  - "출처: 회사 10-Q, Q2 2025, Exhibit 99.1, [SEC EDGAR URL]"
  - "출처: Bloomberg Terminal, 8/15/2025, AAPL US Equity"
  - "출처: FactSet, 8/20/2025, Consensus Estimates Screen"

# XLSX 생성, 편집 및 분석

## 개요

사용자가 .xlsx 파일의 내용을 생성, 편집 또는 분석하도록 요청할 수 있습니다. 다양한 작업에 사용할 수 있는 다양한 도구와 워크플로우가 있습니다.

## 중요 요구 사항

**수식 재계산에 LibreOffice 필요**: `recalc.py` 스크립트를 사용하여 수식 값을 재계산하기 위해 LibreOffice가 설치되어 있다고 가정할 수 있습니다. 스크립트는 첫 실행 시 LibreOffice를 자동으로 구성합니다

## 데이터 읽기 및 분석

### pandas로 데이터 분석
데이터 분석, 시각화 및 기본 작업의 경우 강력한 데이터 조작 기능을 제공하는 **pandas**를 사용하십시오:

```python
import pandas as pd

# Excel 읽기
df = pd.read_excel('file.xlsx')  # 기본값: 첫 번째 시트
all_sheets = pd.read_excel('file.xlsx', sheet_name=None)  # 딕셔너리로 모든 시트

# 분석
df.head()      # 데이터 미리보기
df.info()      # 열 정보
df.describe()  # 통계

# Excel 쓰기
df.to_excel('output.xlsx', index=False)
```

## Excel 파일 워크플로우

## 중요: 하드코딩된 값이 아닌 수식 사용

**항상 Python에서 값을 계산하고 하드코딩하는 대신 Excel 수식을 사용하십시오.** 이렇게 하면 스프레드시트가 동적으로 유지되고 업데이트 가능합니다.

### ❌ 잘못됨 - 계산된 값 하드코딩
```python
# 나쁨: Python에서 계산하고 결과 하드코딩
total = df['Sales'].sum()
sheet['B10'] = total  # 5000을 하드코딩

# 나쁨: Python에서 성장률 계산
growth = (df.iloc[-1]['Revenue'] - df.iloc[0]['Revenue']) / df.iloc[0]['Revenue']
sheet['C5'] = growth  # 0.15를 하드코딩

# 나쁨: 평균에 대한 Python 계산
avg = sum(values) / len(values)
sheet['D20'] = avg  # 42.5를 하드코딩
```

### ✅ 올바름 - Excel 수식 사용
```python
# 좋음: Excel이 합계를 계산하도록 합니다
sheet['B10'] = '=SUM(B2:B9)'

# 좋음: Excel 수식으로 성장률
sheet['C5'] = '=(C4-C2)/C2'

# 좋음: Excel 함수를 사용한 평균
sheet['D20'] = '=AVERAGE(D2:D19)'
```

이것은 모든 계산에 적용됩니다 - 합계, 백분율, 비율, 차이 등. 스프레드시트는 소스 데이터가 변경될 때 재계산할 수 있어야 합니다.

## 일반적인 워크플로우
1. **도구 선택**: 데이터의 경우 pandas, 수식/서식의 경우 openpyxl
2. **생성/로드**: 새 통합 문서를 만들거나 기존 파일을 로드합니다
3. **수정**: 데이터, 수식 및 서식을 추가/편집합니다
4. **저장**: 파일에 씁니다
5. **수식 재계산(수식 사용 시 필수)**: recalc.py 스크립트 사용
   ```bash
   python recalc.py output.xlsx
   ```
6. **오류 확인 및 수정**:
   - 스크립트는 오류 세부 정보가 포함된 JSON을 반환합니다
   - `status`가 `errors_found`인 경우 특정 오류 유형 및 위치에 대해 `error_summary`를 확인하십시오
   - 식별된 오류를 수정하고 다시 재계산하십시오
   - 수정할 일반적인 오류:
     - `#REF!`: 잘못된 셀 참조
     - `#DIV/0!`: 0으로 나누기
     - `#VALUE!`: 수식의 잘못된 데이터 유형
     - `#NAME?`: 인식할 수 없는 수식 이름

### 새 Excel 파일 만들기

```python
# 수식 및 서식을 위한 openpyxl 사용
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment

wb = Workbook()
sheet = wb.active

# 데이터 추가
sheet['A1'] = 'Hello'
sheet['B1'] = 'World'
sheet.append(['Row', 'of', 'data'])

# 수식 추가
sheet['B2'] = '=SUM(A1:A10)'

# 서식 지정
sheet['A1'].font = Font(bold=True, color='FF0000')
sheet['A1'].fill = PatternFill('solid', start_color='FFFF00')
sheet['A1'].alignment = Alignment(horizontal='center')

# 열 너비
sheet.column_dimensions['A'].width = 20

wb.save('output.xlsx')
```

### 기존 Excel 파일 편집

```python
# 수식 및 서식을 보존하기 위해 openpyxl 사용
from openpyxl import load_workbook

# 기존 파일 로드
wb = load_workbook('existing.xlsx')
sheet = wb.active  # 또는 특정 시트의 경우 wb['SheetName']

# 여러 시트 작업
for sheet_name in wb.sheetnames:
    sheet = wb[sheet_name]
    print(f"시트: {sheet_name}")

# 셀 수정
sheet['A1'] = '새 값'
sheet.insert_rows(2)  # 위치 2에 행 삽입
sheet.delete_cols(3)  # 열 3 삭제

# 새 시트 추가
new_sheet = wb.create_sheet('NewSheet')
new_sheet['A1'] = '데이터'

wb.save('modified.xlsx')
```

## 수식 재계산

openpyxl로 생성되거나 수정된 Excel 파일에는 수식이 문자열로 포함되어 있지만 계산된 값은 포함되어 있지 않습니다. 제공된 `recalc.py` 스크립트를 사용하여 수식을 재계산하십시오:

```bash
python recalc.py <excel_file> [timeout_seconds]
```

예:
```bash
python recalc.py output.xlsx 30
```

스크립트:
- 첫 실행 시 LibreOffice 매크로를 자동으로 설정합니다
- 모든 시트의 모든 수식을 재계산합니다
- Excel 오류(#REF!, #DIV/0! 등)에 대해 모든 셀을 스캔합니다
- 자세한 오류 위치 및 개수가 포함된 JSON을 반환합니다
- Linux 및 macOS 모두에서 작동합니다

## 수식 검증 체크리스트

수식이 올바르게 작동하는지 확인하기 위한 빠른 확인:

### 필수 검증
- [ ] **2-3개 샘플 참조 테스트**: 전체 모델을 구축하기 전에 올바른 값을 가져오는지 확인
- [ ] **열 매핑**: Excel 열이 일치하는지 확인(예: 열 64 = BL, BK 아님)
- [ ] **행 오프셋**: Excel 행은 1부터 시작합니다(DataFrame 행 5 = Excel 행 6)

### 일반적인 함정
- [ ] **NaN 처리**: `pd.notna()`로 null 값 확인
- [ ] **맨 오른쪽 열**: FY 데이터는 종종 열 50+
- [ ] **여러 일치**: 첫 번째뿐만 아니라 모든 발생 검색
- [ ] **0으로 나누기**: 수식에서 `/`를 사용하기 전에 분모 확인(#DIV/0!)
- [ ] **잘못된 참조**: 모든 셀 참조가 의도한 셀을 가리키는지 확인(#REF!)
- [ ] **시트 간 참조**: 시트 연결을 위해 올바른 형식(Sheet1!A1) 사용

### 수식 테스트 전략
- [ ] **작게 시작**: 광범위하게 적용하기 전에 2-3개 셀에서 수식 테스트
- [ ] **종속성 확인**: 수식에서 참조하는 모든 셀이 존재하는지 확인
- [ ] **엣지 케이스 테스트**: 0, 음수 및 매우 큰 값 포함

### recalc.py 출력 해석
스크립트는 오류 세부 정보가 포함된 JSON을 반환합니다:
```json
{
  "status": "success",           // 또는 "errors_found"
  "total_errors": 0,              // 총 오류 개수
  "total_formulas": 42,           // 파일의 수식 수
  "error_summary": {              // 오류가 발견된 경우에만 존재
    "#REF!": {
      "count": 2,
      "locations": ["Sheet1!B5", "Sheet1!C10"]
    }
  }
}
```

## 모범 사례

### 라이브러리 선택
- **pandas**: 데이터 분석, 대량 작업 및 간단한 데이터 내보내기에 가장 적합
- **openpyxl**: 복잡한 서식 지정, 수식 및 Excel 관련 기능에 가장 적합

### openpyxl 작업
- 셀 인덱스는 1부터 시작합니다(row=1, column=1은 셀 A1을 나타냄)
- 계산된 값을 읽으려면 `data_only=True` 사용: `load_workbook('file.xlsx', data_only=True)`
- **경고**: `data_only=True`로 열고 저장하면 수식이 값으로 대체되고 영구적으로 손실됩니다
- 큰 파일의 경우: 읽기용 `read_only=True` 또는 쓰기용 `write_only=True` 사용
- 수식은 보존되지만 평가되지 않습니다 - 값을 업데이트하려면 recalc.py 사용

### pandas 작업
- 추론 문제를 피하기 위해 데이터 유형 지정: `pd.read_excel('file.xlsx', dtype={'id': str})`
- 큰 파일의 경우 특정 열 읽기: `pd.read_excel('file.xlsx', usecols=['A', 'C', 'E'])`
- 날짜를 적절하게 처리: `pd.read_excel('file.xlsx', parse_dates=['date_column'])`

## 코드 스타일 지침
**중요**: Excel 작업을 위한 Python 코드를 생성할 때:
- 불필요한 주석 없이 최소한의 간결한 Python 코드를 작성하십시오
- 장황한 변수 이름과 중복 작업을 피하십시오
- 불필요한 print 문을 피하십시오

**Excel 파일 자체의 경우**:
- 복잡한 수식 또는 중요한 가정이 있는 셀에 주석을 추가하십시오
- 하드코딩된 값에 대한 데이터 소스를 문서화하십시오
- 주요 계산 및 모델 섹션에 대한 메모를 포함하십시오
