# 무상 CLAIM 데이터 정규화 프로젝트

## 📌 프로젝트 개요

본 프로젝트는 2024년 WQ Claim List 데이터에서 **무상CLAIM** 데이터를 추출하여 데이터베이스 정규화(3NF)를 수행하고, 관계형 데이터베이스로 구조화한 프로젝트입니다.

### 주요 목표
- ✅ 무상CLAIM 데이터 추출 (1,650건)
- ✅ 데이터베이스 제3정규화(3NF) 수행
- ✅ ERD 다이어그램 생성
- ✅ 데이터 통계 분석 및 시각화

---

## 📂 프로젝트 구조

```
quality_graph/
├── data/
│   └── 2024_1월-12월_WQ_Claim List_QMS_Issue_List_20250308_분석_매크로_v3.4_CV사양설명추가.csv
├── output/
│   ├── claim_normalized.db           # SQLite 데이터베이스
│   ├── claim_erd.png                 # ERD 다이어그램
│   ├── claim_statistics.png          # 통계 시각화 차트
│   ├── table_statistics.txt          # 테이블 통계 정보
│   ├── ERD_설명서.md                 # 상세 설명 문서
│   └── graphs/                       # 그래프 분석 결과
│       ├── 1_claim_flow_graph.png    # 클레임 흐름 그래프
│       ├── 2_unit_symptom_network.png # Unit-현상구분 네트워크
│       ├── 3_model_claim_network.png  # 기종-클레임 패턴 네트워크
│       ├── 4_knowledge_graph.png      # 지식 그래프
│       ├── 5_circular_hierarchy.png   # 원형 계층 그래프
│       ├── 6_sankey_flow.png          # Sankey 흐름 그래프
│       ├── graph_statistics.txt       # 그래프 통계
│       └── 그래프_분석_가이드.md       # 그래프 상세 설명
├── normalize_claims.py               # 메인 정규화 스크립트
├── query_database.py                 # 데이터베이스 조회 스크립트
├── create_graph_visualizations.py    # 그래프 생성 스크립트
└── README.md                         # 본 문서
```

---

## 🚀 실행 방법

### 1. 환경 설정

```bash
# 필요한 패키지 설치
pip install pandas matplotlib graphviz
```

### 2. 데이터 정규화 및 ERD 생성

```bash
# 무상CLAIM 데이터 추출 및 정규화
python normalize_claims.py
```

**실행 결과:**
- ✅ 9개의 정규화된 테이블 생성
- ✅ SQLite 데이터베이스 파일 생성
- ✅ ERD 다이어그램 이미지 생성
- ✅ 통계 차트 및 문서 생성

### 3. 데이터베이스 조회 및 분석

```bash
# 데이터베이스 스키마 및 샘플 데이터 조회
python query_database.py
```

**출력 내용:**
- 테이블별 스키마 정보
- 샘플 데이터 (각 테이블 상위 5개)
- 관계형 조인 쿼리 예제 (4개)

### 4. 그래프 생성 및 시각화

```bash
# 6가지 유형의 그래프 생성
python create_graph_visualizations.py
```

**생성되는 그래프:**
1. ✅ 클레임 흐름 그래프 (현상 → 원인 → 조치)
2. ✅ Unit-현상구분 네트워크
3. ✅ 기종-클레임 패턴 네트워크
4. ✅ 지식 그래프 (계층적 관계)
5. ✅ 원형 계층 그래프
6. ✅ Sankey 흐름 그래프 (기종군 → 현상 → 원인)

**출력 위치:** `output/graphs/` 디렉토리

---

## 🗄️ 데이터베이스 구조

### 정규화된 테이블 (총 9개)

| 번호 | 테이블명 | 행 수 | 설명 |
|------|----------|-------|------|
| 1 | 기종군 | 8 | 기종 최상위 분류 (PK: 기종군_ID) |
| 2 | 기종명 | 204 | 모델명 (FK: 기종군_ID) |
| 3 | 기종호기 | 1,208 | 장비 시리얼 번호 (FK: 기종명_ID) |
| 4 | Unit | 19 | 주요 유닛 분류 (PK: Unit_ID) |
| 5 | Sub_Unit | 92 | 세부 유닛 (FK: Unit_ID) |
| 6 | 현상구분_코드 | 9 | 클레임 현상 마스터 |
| 7 | 원인구분_코드 | 8 | 클레임 원인 마스터 |
| 8 | 조치구분_코드 | 7 | 클레임 조치 마스터 |
| 9 | 무상_Claim | 1,654 | 메인 트랜잭션 테이블 (5개 FK) |

### ERD 관계도

```
기종군 (1:N) → 기종명 (1:N) → 기종호기 (1:N) → 무상_Claim
Unit (1:N) → Sub_Unit (1:N) → 무상_Claim
현상구분_코드 (1:N) → 무상_Claim
원인구분_코드 (1:N) → 무상_Claim
조치구분_코드 (1:N) → 무상_Claim
```

자세한 ERD는 `output/claim_erd.png` 파일 참조

---

## 📊 그래프 분석 결과

### 생성된 그래프 유형

| 번호 | 그래프 유형 | 노드 수 | 엣지 수 | 주요 용도 |
|------|-------------|---------|---------|-----------|
| 1 | 클레임 흐름 그래프 | 24 | 92 | 현상→원인→조치 프로세스 분석 |
| 2 | Unit-현상구분 네트워크 | 21 | 39 | Unit별 취약 현상 파악 |
| 3 | 기종-클레임 패턴 네트워크 | 25 | 35 | 기종별 품질 이슈 분석 |
| 4 | 지식 그래프 | 77 | 197 | 계층적 관계 및 온톨로지 |
| 5 | 원형 계층 그래프 | 25 | 24 | 전체 구조 개요 파악 |
| 6 | Sankey 흐름 그래프 | 21 | 53 | 기종군→현상→원인 흐름 |

### 주요 그래프 인사이트

#### 1. 클레임 흐름 그래프
- **가장 중요한 노드**: 조립 이상 (중심성 0.6522)
- **주요 흐름**: 작동 불량 → 부품 불량 → 부품 교환
- **활용**: 클레임 처리 프로세스 최적화

#### 2. Unit-현상구분 네트워크
- **최고 중심성**: 작동 불량 (0.6000)
- **다양한 연결**: 성능 이상 (0.5000)
- **활용**: Unit별 예방 정비 계획 수립

#### 3. 기종-클레임 패턴 네트워크
- **빈번한 패턴**: 작동 불량 + 부품 불량
- **다양한 패턴 기종**: DNM 5700, MYNX 6500/50 II
- **활용**: 기종별 맞춤형 품질 개선

자세한 내용은 `output/graphs/그래프_분석_가이드.md` 참조

---

## 📊 주요 분석 결과

### 1. 기종별 클레임 Top 5
1. **NHM 6300** (Horizontal M/C): 68건
2. **DNM 5700** (Vertical M/C): 68건
3. **MYNX 6500/50 II** (Vertical M/C): 64건
4. **DNM 4500** (Vertical M/C): 57건
5. **MYNX 6500 II** (Vertical M/C): 47건

### 2. Unit별 클레임 Top 3
1. **Elec._Ctrl / Electric Box**: 174건
2. **Cover / Splash Guard**: 145건
3. **ATC_Magazine / Tool Magazine**: 90건

### 3. 주요 클레임 패턴
- **작동 불량 + 부품 불량 + 부품 교환**: 214건 (최다)
- **성능 이상 + 부품 불량 + 부품 교환**: 127건
- **소음_이음_진동 + 부품 불량 + 부품 교환**: 76건

---

## 💡 SQL 쿼리 예제

### 예제 1: 기종별 클레임 집계

```sql
SELECT 
    기종군.기종군,
    기종명.기종명,
    COUNT(무상_Claim.Claim_ID) as 클레임수
FROM 무상_Claim
JOIN 기종호기 ON 무상_Claim.기종호기_ID = 기종호기.기종호기_ID
JOIN 기종명 ON 기종호기.기종명_ID = 기종명.기종명_ID
JOIN 기종군 ON 기종명.기종군_ID = 기종군.기종군_ID
GROUP BY 기종군.기종군, 기종명.기종명
ORDER BY 클레임수 DESC;
```

### 예제 2: Unit별 현상구분 분석

```sql
SELECT 
    Unit.Unit,
    현상구분_코드.현상구분,
    COUNT(*) as 클레임수
FROM 무상_Claim
JOIN Sub_Unit ON 무상_Claim.Sub_Unit_ID = Sub_Unit.Sub_Unit_ID
JOIN Unit ON Sub_Unit.Unit_ID = Unit.Unit_ID
JOIN 현상구분_코드 ON 무상_Claim.현상구분_ID = 현상구분_코드.현상구분_ID
GROUP BY Unit.Unit, 현상구분_코드.현상구분
ORDER BY 클레임수 DESC;
```

### 예제 3: 특정 기종의 전체 클레임 상세 조회

```sql
SELECT 
    c.Claim_ID,
    mg.기종군,
    mn.기종명,
    ms.기종호기,
    u.Unit,
    su.[Sub Unit],
    s.현상구분,
    ca.원인구분,
    ac.조치구분
FROM 무상_Claim c
JOIN 기종호기 ms ON c.기종호기_ID = ms.기종호기_ID
JOIN 기종명 mn ON ms.기종명_ID = mn.기종명_ID
JOIN 기종군 mg ON mn.기종군_ID = mg.기종군_ID
JOIN Sub_Unit su ON c.Sub_Unit_ID = su.Sub_Unit_ID
JOIN Unit u ON su.Unit_ID = u.Unit_ID
JOIN 현상구분_코드 s ON c.현상구분_ID = s.현상구분_ID
JOIN 원인구분_코드 ca ON c.원인구분_ID = ca.원인구분_ID
JOIN 조치구분_코드 ac ON c.조치구분_ID = ac.조치구분_ID
WHERE mn.기종명 = 'DNM 4500';
```

---

## 📈 생성된 파일

| 파일명 | 크기 | 설명 |
|--------|------|------|
| `claim_normalized.db` | 112KB | SQLite 데이터베이스 |
| `claim_erd.png` | 52KB | ERD 다이어그램 이미지 |
| `claim_statistics.png` | 556KB | 통계 시각화 차트 |
| `table_statistics.txt` | 4.5KB | 테이블 통계 정보 |
| `ERD_설명서.md` | - | 상세 설명 문서 |

---

## 🎯 정규화 달성 사항

### ✅ 제1정규화 (1NF)
- 모든 속성을 원자값으로 분리
- 각 테이블에 기본키 설정
- 반복 그룹 제거

### ✅ 제2정규화 (2NF)
- 부분 함수 종속성 제거
- 기종 계층 구조 분리 (기종군 → 기종명 → 기종호기)
- Unit 계층 구조 분리 (Unit → Sub Unit)

### ✅ 제3정규화 (3NF)
- 이행적 함수 종속성 제거
- 코드 마스터 테이블 분리 (현상구분, 원인구분, 조치구분)
- 모든 비키 속성이 기본키에만 종속

---

## 🔧 기술 스택

- **언어**: Python 3
- **데이터 처리**: Pandas
- **데이터베이스**: SQLite 3
- **시각화**: Matplotlib, Graphviz
- **정규화 수준**: 3NF (Third Normal Form)

---

## 📋 데이터 처리 통계

- **원본 데이터**: 11,728행 (전체 Claim)
- **필터링 데이터**: 1,650행 (무상CLAIM만)
- **사용 컬럼**: 8개
- **생성 테이블**: 9개
- **정규화 수준**: 3NF
- **총 레코드 수**: 3,249개 (9개 테이블 합계)

---

## 🚀 향후 확장 가능성

### 1. 추가 분석 항목
- 시계열 분석 (접수일, 조치완료일 기준)
- 비용 분석 (부품비, 공임, 운송비)
- 반복 클레임 패턴 분석
- 지역별 클레임 분포

### 2. 데이터베이스 확장
- PostgreSQL, MySQL 등으로 마이그레이션
- 인덱스 최적화
- 뷰(View) 생성으로 복잡한 쿼리 단순화
- 저장 프로시저 구현

### 3. 시각화 개선
- 대시보드 개발 (Tableau, Power BI, Streamlit)
- 실시간 모니터링 시스템
- 인터랙티브 차트 구현

### 4. 머신러닝 적용
- 클레임 발생 예측 모델
- 이상 패턴 감지
- 자동 분류 시스템

---

## 📞 문의 및 지원

프로젝트에 대한 문의사항이나 개선 제안이 있으시면 이슈를 등록해주세요.

---

## 📄 라이선스

본 프로젝트는 내부 데이터 분석 용도로 제작되었습니다.

---

**생성일**: 2024년 11월 21일  
**버전**: 1.0  
**상태**: ✅ 완료

