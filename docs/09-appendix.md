# 09. 부록 (Appendix)

> **TL;DR**
> 1. AutoMarket 전 문서에서 사용된 핵심 용어 17개를 한 곳에 정의하여, 처음 읽는 독자도 개념 혼선 없이 이해할 수 있도록 한다.
> 2. v2 → v3 → v5 → v6으로 이어진 아키텍처 진화의 각 단계별 이유와 폐기 이유를 명시적으로 기록한다.
> 3. 12개 SaaS 각각의 경쟁 구도와 기술 참고 자료를 색인과 함께 제공하여 빠른 탐색을 돕는다.

---

## 1. 용어 정의 (Glossary)

아래 용어는 AutoMarket 전 문서에 걸쳐 공통으로 사용된다. 기술 용어는 영문 원어와 함께 표기한다.

| 용어 | 영문 | 정의 |
|------|------|------|
| 신경계 | Nervous System | 각 노드에 내장된 실시간 피드백 루프. 성과 지표(KPI)를 감시하고 내부 파라미터를 자동 조정하는 자율 제어 메커니즘 |
| TTR | Time-to-Revenue | 트렌드 감지부터 첫 매출 발생까지 소요되는 시간. AutoMarket의 목표 기준치는 24시간 |
| 기회 스코어 | Opportunity Score | 상품의 시장 기회를 0~100 범위로 수치화한 복합 지표. 검색량·경쟁 강도·마진율·트렌드 속도를 가중 합산하여 산출 |
| Trust Escalation | Trust Escalation | AI 자동화 레벨(Autopilot / Copilot / Human-led)을 성과 데이터 기반으로 자동 승격하거나 롤백하는 메커니즘 |
| Autopilot | Autopilot | AI가 자율 실행하고 결과를 사후 보고하는 자동화 레벨. 전체 태스크의 약 60%를 담당 |
| Copilot | Copilot | AI가 행동 계획을 제안하고 사람이 최종 승인하는 자동화 레벨. 전체 태스크의 약 25%를 담당 |
| Human-led | Human-led | 사람이 의사결정을 주도하고 AI가 데이터·초안을 보조하는 자동화 레벨. 전체 태스크의 약 15%를 담당 |
| Arbitration Layer | Arbitration Layer | 복수의 노드가 동일 리소스(예: 가격, 예산)를 동시에 조정하려 할 때 충돌을 해소하는 경량 조정 레이어 |
| 듀얼 엔진 | Dual Engine | Node 3(콘텐츠 공급망)의 이원 생산 구조. 내부 AI 생성기(AI Generator)와 외부 파트너 네트워크(Partner Network)가 병렬 가동 |
| PMF | Product-Market Fit | 제품이 시장 수요에 정확히 부합하는 상태. AutoMarket에서는 SaaS별 단독 가치 테스트 통과 기준으로 활용 |
| LTV | Lifetime Value | 고객 생애 가치. 한 고객이 서비스 이용 기간 동안 창출하는 총 예상 수익 |
| ROAS | Return on Ad Spend | 광고 수익률. 광고비 1원당 발생하는 매출액 비율 |
| MOQ | Minimum Order Quantity | 공급사가 허용하는 최소 주문 수량. SourceMatch의 파트너 필터링 기준 중 하나 |
| MRR | Monthly Recurring Revenue | 월간 반복 매출. SaaS 구독 사업의 핵심 성장 지표 |
| MAU | Monthly Active Users | 월간 활성 사용자 수. 플랫폼 채택률과 리텐션을 동시에 반영하는 지표 |
| Cold Start | Cold Start | 초기 데이터 부족으로 AI 모델이 정확한 예측·추천을 수행하지 못하는 상태. 해결 방법은 [07-risks-and-mitigations.md](./07-risks-and-mitigations.md) 참조 |
| UGC | User Generated Content | 사용자 생성 콘텐츠. 리뷰, 사용 후기 영상, SNS 게시물 등을 포함하며 ReviewOps·InfluBridge가 수집·큐레이션 |

---

## 2. 아키텍처 진화 전체 히스토리

AutoMarket 아키텍처는 네 차례의 주요 버전을 거쳐 현재의 4노드 구조(v6)에 도달했다. 각 버전의 설계 의도와 폐기 이유를 아래에 명시한다. 최종 v6 상세 명세는 [02-architecture.md](./02-architecture.md)를 참조한다.

### v2 — 9노드: 프로세스 복사 (Process Mirroring)

기존 이커머스 운영 부서 구조를 그대로 디지털화한 버전이다.

- **노드 구성 (9개)**: 인텔리전스, 소싱, 상품기획, 고객 CRM, 브랜딩, 콘텐츠, 소셜프루프, 유통채널, 마케팅자동화
- **설계 의도**: 기존 업무 방식에 AI를 점진적으로 삽입하여 초기 거부감을 최소화
- **폐기 이유**: 노드 간 책임 중복이 심각했고(예: 소싱과 상품기획이 동일 데이터를 각자 조회), AI 실행 단위(Agent)와 부서 단위가 불일치하여 자동화 효율이 낮았음

### v3 — 5노드: 제1원리 압축 (First-Principles Compression)

Supply / Demand / Match 세 가지 본질적 요소를 기준으로 전면 재구성한 버전이다. 설계 철학의 상세 배경은 [01-philosophy.md](./01-philosophy.md)를 참조한다.

- **통합 내역**: 인텔리전스 + 소싱 + 상품기획 → 기회 발견 / 고객 CRM + 브랜딩 → 고객 인텔리전스 / 콘텐츠, 유통, 마케팅 → 각각 독립 노드 유지
- **폐기 이유**: 소셜 프루프(리뷰·인플루언서)의 독립적 사업 가치와 운영 복잡도를 과소평가. 콘텐츠 노드에 흡수되면서 전담 KPI 추적이 불가능해짐

### v5 — 6노드: 소셜프루프 분리 (Social Proof Extraction)

소셜 프루프 엔진을 별도 노드로 독립시킨 버전이다.

- **추가 노드**: 소셜프루프 엔진 (리뷰 관리, 인플루언서 매칭, UGC 큐레이션 전담)
- **폐기 이유**: 콘텐츠 생산과 소셜 프루프가 실질적으로 동일한 에셋(영상·이미지·카피)을 공유함을 확인. 별도 노드로 운영 시 에셋 동기화 오버헤드가 발생하고, 두 노드의 KPI가 상호 충돌하는 경우가 빈번했음

### v6 — 4노드: 최종 아키텍처 (Final Architecture)

콘텐츠 생산과 소셜 프루프를 "콘텐츠 공급망"으로 통합하고, 유통과 마케팅을 "채널 오케스트레이터"로 합쳐 최종 4노드 구조를 확정했다.

| 노드 번호 | 노드명 | 담당 SaaS |
|-----------|--------|-----------|
| Node 1 | 기회 발견 (Opportunity Discovery) | TrendRadar, SourceMatch, MarginSim |
| Node 2 | 고객 인텔리전스 (Customer Intelligence) | PersonaAI, BrandForge, ChurnGuard |
| Node 3 | 콘텐츠 공급망 (Content Supply Chain) | PageFactory, SocialSpray, ReviewOps, InfluBridge |
| Node 4 | 채널 오케스트레이터 (Channel Orchestrator) | ChannelSync, AdPilot |

---

## 3. 경쟁사 매핑 (Competitive Landscape)

12개 SaaS 각각의 주요 경쟁사와 AutoMarket의 차별화 포인트를 정리한다. 각 제품의 상세 스펙은 [04-saas-products.md](./04-saas-products.md)를 참조한다.

| SaaS | 주요 경쟁사 | AutoMarket 차별화 포인트 |
|------|-------------|--------------------------|
| TrendRadar | 셀러마스터, 아이템스카우트 | 키워드 순위 모니터링 중심 → AI 수요 예측 + 기회 스코어 자동 산출 |
| SourceMatch | 1688, 도매꾹 | 셀러가 수동 탐색 → AI가 스펙·MOQ·마진을 기준으로 공급사 자동 매칭 |
| MarginSim | 엑셀/스프레드시트 | 수작업 계산 → 채널별 수수료 DB 내장, Go/No-go 판단 자동화 |
| PersonaAI | 채널톡, Amplitude | CS 처리 및 일반 분석 중심 → 이커머스 판매 전략 수립에 특화된 고객 세분화 |
| BrandForge | 브랜딩 에이전시 | 수백만 원 비용·수주 대기 → AI로 브랜드 아이덴티티 자동 생성 |
| ChurnGuard | 채널톡 이탈 방지 기능 | 범용 이탈 감지 → 이커머스 구매 주기 기반 LTV 예측 및 선제 개입 |
| PageFactory | 미리캔버스, Canva | 범용 디자인 도구 → 이커머스 상세페이지 전문 템플릿 + A/B 테스트 자동화 |
| SocialSpray | Buffer, Hootsuite, Later | 글로벌 범용 SNS 스케줄링 → 네이버 블로그·카카오·쿠팡 등 국내 이커머스 채널 특화 |
| ReviewOps | (시장 공백) | 리뷰 수집·답변·분석 통합 자동화를 제공하는 최초 전용 솔루션 |
| InfluBridge | 크리에이터링크, 레뷰 | 인플루언서 매칭 단계에서 종료 → 매칭 + 캠페인 실행 + ROI 추적 풀스택 제공 |
| ChannelSync | 사방넷, 플레이오토 | 주문·재고 관리 중심 → 채널별 성과 분석 및 채널 믹스 최적화까지 확장 |
| AdPilot | NHN ACE, 와이즈트래커 | 광고 분석 리포팅 중심 → 분석 결과를 입력 삼아 입찰·소재 교체 자동 실행 |

---

## 4. 기술 참고 자료 (Technical References)

| 분류 | 참고 자료 | 적용 영역 |
|------|-----------|-----------|
| 이벤트 기반 아키텍처 | Martin Fowler - *Event-Driven Architecture* (martinfowler.com) | 노드 간 비동기 메시지 버스 설계. 상세는 [05-data-architecture.md](./05-data-architecture.md) 참조 |
| AI Agent 설계 | LangChain, AutoGen 프레임워크 공식 문서 | 각 노드의 Agent 오케스트레이션 패턴 및 Tool 호출 설계 |
| Trust Escalation | Netflix - Confidence-based A/B Testing 모델 | 자동화 레벨 승격/롤백 임계값 설정 방법론. 상세는 [03-automation-design.md](./03-automation-design.md) 참조 |
| 이커머스 데이터 수집 | 네이버 데이터랩 API, 쿠팡 파트너스 API 공식 문서 | TrendRadar의 실시간 검색 트렌드 및 판매 데이터 수집 파이프라인 |
| LLM 활용 | Claude API (Anthropic), OpenAI API 공식 문서 | BrandForge·PageFactory·AdPilot 등 생성형 AI 기능 전반 |

---

## 5. 문서 색인 (Document Index)

AutoMarket 문서는 총 10개로 구성된다. 아래 순서로 읽으면 전체 맥락을 가장 빠르게 파악할 수 있다.

| # | 파일 | 제목 | 주요 내용 |
|---|------|------|-----------|
| 00 | [00-overview.md](./00-overview.md) | AutoMarket 플랫폼 총괄 개요 | 플랫폼 전체 구조, 4노드·12 SaaS 요약, 핵심 수치 |
| 01 | [01-philosophy.md](./01-philosophy.md) | 설계 철학 | Supply/Demand/Match 제1원리, 버전 진화 배경 |
| 02 | [02-architecture.md](./02-architecture.md) | v6 아키텍처 상세 명세 | 4노드 구조, Arbitration Layer, 신경계 메커니즘 |
| 03 | [03-automation-design.md](./03-automation-design.md) | AI 자동화 & Human-in-the-Loop 설계 | Trust Escalation, Autopilot/Copilot/Human-led 비율 |
| 04 | [04-saas-products.md](./04-saas-products.md) | 12개 SaaS 제품 카탈로그 | 각 SaaS의 기능 정의, 단독 가치 테스트 기준, 시너지 |
| 05 | [05-data-architecture.md](./05-data-architecture.md) | 데이터 아키텍처 & 통합 설계 | 공통 도메인 엔티티, 이벤트 버스, 피드백 루프 |
| 06 | [06-roadmap.md](./06-roadmap.md) | 실행 로드맵 & GTM 전략 | 단계별 출시 계획, 목표 MRR·MAU, GTM 채널 전략 |
| 07 | [07-risks-and-mitigations.md](./07-risks-and-mitigations.md) | 리스크 분석 및 대응 전략 | Cold Start·LLM 비용·경쟁 과밀화 등 주요 리스크와 완화 방안 |
| 08 | *(작성 예정)* | 운영 플레이북 | 일별·주별·월별 운영 체크리스트, 장애 대응 런북 |
| 09 | [09-appendix.md](./09-appendix.md) | 부록 | 용어 정의, 아키텍처 히스토리, 경쟁사 매핑, 기술 참고 자료 |

---

*최종 수정: 2026-03-08 | 관련 문서: [00-overview.md](./00-overview.md) · [02-architecture.md](./02-architecture.md) · [04-saas-products.md](./04-saas-products.md)*
