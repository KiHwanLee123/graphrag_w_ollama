---
config:
  layout: elk
---
flowchart TB
 subgraph PRODUCT["제품 계층"]
        MG["기종군 노드<br>-----------<br>name: Vertical M/C<br>code: VMC"]
        M["기종명 노드<br>-----------<br>name: DNM 4500<br>model_code: DNM4500<br>spec_number: DNM4500-F0MP-0-K30"]
        U1["Unit 노드<br>-----------<br>name: Column_Bed_Feed<br>category: Mechanical"]
        U2["Unit 노드<br>-----------<br>name: Elec._Ctrl<br>category: Electrical"]
        SU1["SubUnit 노드<br>-----------<br>name: Z-Axis<br>description: Z축 이송계"]
        SU2["SubUnit 노드<br>-----------<br>name: Electric Box<br>description: 전기 제어반"]
  end
 subgraph CATEGORY["현상-원인-조치 구분"]
        SYM_CAT1["현상구분 노드<br>-----------<br>name: 소음_이음_진동<br>occurrence_count: 45"]
        SYM_CAT2["현상구분 노드<br>-----------<br>name: 전기_제어 이상<br>occurrence_count: 32"]
        CAU_CAT1["원인구분 노드<br>-----------<br>name: 부품 불량<br>responsibility: 벤더"]
        CAU_CAT2["원인구분 노드<br>-----------<br>name: 설계 오류<br>responsibility: 설계부서"]
        ACT_CAT1["조치구분 노드<br>-----------<br>name: 부품 교환<br>avg_cost: 250000"]
        ACT_CAT2["조치구분 노드<br>-----------<br>name: 현상/원인 진단<br>avg_cost: 50000"]
  end
 subgraph ORG["조직 관련"]
        PROD1["생산처 노드<br>-----------<br>name: 대영<br>code: DY<br>total_claims: 120"]
        PROD2["생산처 노드<br>-----------<br>name: TC3직<br>code: TC3<br>total_claims: 89"]
        CUST1["고객사 노드<br>-----------<br>name: 원테크<br>code: WT<br>total_claims: 35"]
        CUST2["고객사 노드<br>-----------<br>name: 한맥토탈시스템<br>code: HM<br>total_claims: 28"]
        SVC1["서비스지정점 노드<br>-----------<br>name: 아이씨에스ICS<br>code: ICS<br>service_count: 89"]
        SVC2["서비스지정점 노드<br>-----------<br>name: 토탈씨엠에스<br>code: TCMS<br>service_count: 67"]
        SVCM1["서비스맨 노드<br>-----------<br>name: MKRC0561 전원봉<br>code: MKRC0561<br>handled_count: 150"]
        SVCM2["서비스맨 노드<br>-----------<br>name: MKRC0049 진상동<br>code: MKRC0049<br>handled_count: 120"]
  end
 subgraph PARTS["부품 관련"]
        PART1["부품 노드<br>-----------<br>material_code: 140101-01392<br>material_name: BEARING BALL<br>unit_price: 50485<br>currency: KRW"]
        PART2["부품 노드<br>-----------<br>material_code: EMAGC0510K<br>material_name: CONTACTOR<br>unit_price: 77853<br>currency: KRW"]
        VENDOR1["벤더 노드<br>-----------<br>name: 셰플러코리아유<br>code: SCHAEFFLER<br>supply_count: 230"]
        VENDOR2["벤더 노드<br>-----------<br>name: 진주일렉콤<br>code: JINJU<br>supply_count: 156"]
  end
    MG -- HAS_MODEL --> M
    M -- HAS_UNIT --> U1 & U2
    U1 -- HAS_SUBUNIT --> SU1
    U2 -- HAS_SUBUNIT --> SU2
    SU1 -- SHOWS_SYMPTOM --> SYM_CAT1
    SU2 -- SHOWS_SYMPTOM --> SYM_CAT2
    SYM_CAT1 -- CAUSED_BY --> CAU_CAT1
    SYM_CAT2 -- CAUSED_BY --> CAU_CAT2
    CAU_CAT1 -- RESOLVED_BY --> ACT_CAT1
    CAU_CAT2 -- RESOLVED_BY --> ACT_CAT2
    C1["클레임 인스턴스 노드<br>========================<br>id: MTS2-24018036<br>noti_no: 302384151<br>-----------<br>shipment_date: 2022-07-03<br>installation_date: 2022-07-04<br>warranty_start: 2022-07-04<br>claim_date: 2024-04-30<br>resolution_date: 2024-06-04<br>elapsed_days: 666<br>response_days: 35<br>-----------<br>cost_parts: 201940<br>cost_labor: 0<br>cost_total: 201940<br>-----------<br>is_recurring: false<br>is_critical: false<br>warranty_status: out<br>region: Korea"] -- OF_MODEL --> M
    C1 -- OCCURRED_IN_UNIT --> U1
    C1 -- OCCURRED_IN_SUBUNIT --> SU1
    C1 -- HAS_현상 --> SYM1["현상 노드<br>-----------<br>text: Z축 베어링 나가<br>소음 발생. 공임 유상.<br>충돌이 없을 경우<br>부품 무상 전달.<br>-----------<br>embedding: vector 1536"]
    C1 -- HAS_원인 --> CAU1["원인 노드<br>-----------<br>text: 베어링소음으로<br>부품신청<br>-----------<br>embedding: vector 1536"]
    C1 -- HAS_조치내용 --> ACT1["조치내용 노드<br>-----------<br>text: Z축 베어링 나가<br>소음 발생. 공임 유상.<br>충돌이 없을 경우<br>부품 무상...<br>-----------<br>embedding: vector 1536"]
    C2["클레임 인스턴스 노드<br>========================<br>id: MTS2-24016266<br>noti_no: 302382954<br>-----------<br>claim_date: 2024-04-19<br>resolution_date: 2024-05-31<br>elapsed_days: 671<br>-----------<br>cost_total: 335887<br>warranty_status: out"] -- OF_MODEL --> M
    C2 -- OCCURRED_IN_UNIT --> U2
    C2 -- OCCURRED_IN_SUBUNIT --> SU2
    C2 -- HAS_현상 --> SYM2["현상 노드<br>-----------<br>text: 스핀들 회전 시<br>스핀들 서보 알람발생<br>-----------<br>embedding: vector 1536"]
    C2 -- HAS_원인 --> CAU2["원인 노드<br>-----------<br>text: 메인, 서브, 밀링스핀들<br>저속라인 마그네트<br>스위치 소착되어<br>서보알람 발생<br>-----------<br>embedding: vector 1536"]
    C2 -- HAS_조치내용 --> ACT2["조치내용 노드<br>-----------<br>text: 4/22 업체 방문하여<br>상태 점검 / 장비 전원 후<br>머신래디시 스핀들<br>서보알람 발생...<br>메인,서브,밀링 스핀들<br>저속라인 마그네트<br>스위치 교체 완료<br>-----------<br>embedding: vector 1536"]
    SYM1 -- CATEGORIZED_AS --> SYM_CAT1
    SYM2 -- CATEGORIZED_AS --> SYM_CAT2
    CAU1 -- CATEGORIZED_AS --> CAU_CAT1
    CAU2 -- CATEGORIZED_AS --> CAU_CAT2
    ACT1 -- CATEGORIZED_AS --> ACT_CAT1
    ACT2 -- CATEGORIZED_AS --> ACT_CAT2
    C1 -- PRODUCED_BY --> PROD1
    C1 -- OWNED_BY --> CUST1
    C1 -- SERVICED_BY --> SVC1
    C1 -- HANDLED_BY --> SVCM1
    C2 -- PRODUCED_BY --> PROD2
    C2 -- OWNED_BY --> CUST2
    C2 -- SERVICED_BY --> SVC2
    C2 -- HANDLED_BY --> SVCM2
    C1 -- USES_PART<br>quantity: 4<br>cost: 201940 --> PART1
    C2 -- USES_PART<br>quantity: 3<br>cost: 233558 --> PART2
    PART1 -- SUPPLIED_BY --> VENDOR1
    PART2 -- SUPPLIED_BY --> VENDOR2

     MG:::product
     M:::product
     U1:::product
     U2:::product
     SU1:::product
     SU2:::product
     SYM_CAT1:::category
     SYM_CAT2:::category
     CAU_CAT1:::category
     CAU_CAT2:::category
     ACT_CAT1:::category
     ACT_CAT2:::category
     PROD1:::org
     PROD2:::org
     CUST1:::org
     CUST2:::org
     SVC1:::org
     SVC2:::org
     SVCM1:::org
     SVCM2:::org
     PART1:::part
     PART2:::part
     VENDOR1:::part
     VENDOR2:::part
     C1:::claim
     SYM1:::text_node
     CAU1:::text_node
     ACT1:::text_node
     C2:::claim
     SYM2:::text_node
     CAU2:::text_node
     ACT2:::text_node
    classDef product fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    classDef category fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    classDef text_node fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    classDef claim fill:#f3e5f5,stroke:#7b1fa2,stroke-width:3px
    classDef org fill:#fff9c4,stroke:#f57f17,stroke-width:2px
    classDef part fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
