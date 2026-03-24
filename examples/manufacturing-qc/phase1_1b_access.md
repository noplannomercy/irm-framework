# Phase 1 1-B — 접근 배치

> 도메인: 제조/품질관리
> 단계: Phase 1 1-B
> 상태: 완료
> 입력: phase0_step3_query_mapping.md (접근 포함 질의 #10~20, #21~25)
> 기준: IRM Framework v10

---

## 1-B-1: 시스템 목록

| # | 시스템명 | 접근 방식 | CLI 가능 여부 | 승격 우선순위 |
|---|---------|---------|:----------:|:----------:|
| 1 | MES (제조실행시스템) | REST API | O | ★★★ |
| 2 | 품질 DB | PostgreSQL 직접 연결 | O | ★★★ |
| 3 | ERP (SAP) | REST API | O | ★★ |
| 4 | 설비 모니터링 | IoT 게이트웨이 API | O | ★★★ |
| 5 | LIMS (시험분석) | REST API | O | ★★ |
| 6 | SPC 시스템 | API | O | ★★ |
| 7 | DMS (문서관리) | REST API | O | ★ |

---

## 1-B-2: 접근 요구사항 도출

| # | 질의 | 필요한 동작 | CLI 설계 힌트 | 승격 우선순위 |
|---|------|---------|------------|:----------:|
| 1 | 현재 생산 라인 상태 어때? | MES 라인 상태 조회 | `mes line-status --line {line}` | ★★★ |
| 2 | 이 로트 검사 결과 알려줘 | 품질DB 로트별 검사 조회 | `quality inspect-result --lot {lot}` | ★★★ |
| 3 | 이 자재 입고 현황 어때? | ERP 자재 입고 조회 | `erp material-receipt --material {id}` | ★★ |
| 4 | 이 설비 현재 온도/압력 알려줘 | 설비 모니터링 실시간 조회 | `equip status --id {equip_id}` | ★★★ |
| 5 | 이 시료 시험 결과 나왔어? | LIMS 시험 결과 조회 | `lims test-result --sample {id}` | ★★ |
| 6 | 이 공정 Cpk 얼마야? | SPC 공정능력 조회 | `spc cpk --process {process} --period {period}` | ★★ |
| 7 | 이 문서 최신 버전 찾아줘 | DMS 문서 검색 | `dms get-latest --doc {doc_id}` | ★ |

**승격 우선순위 기준:**
- ★★★: MES, 품질DB, 설비 모니터링 — 불량 대응·검사 시 매번 호출. 즉시 CLI 표준화
- ★★: ERP, LIMS, SPC — 검사 절차에서 호출되나 빈도 중간
- ★: DMS — 필요 시 조회

---

> **다음 단계:** Phase 1 1-C — 실행(확정) 배치
