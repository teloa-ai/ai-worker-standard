# AI Worker Standard 1.0

**AI Worker, AI Team, AI Organization을 위한 공개 정의 표준입니다.**

**Max Luo, Morgan Chen｜Teloa 공동 창업자** · 2026년 10월 18일

[English](README.md) · [简体中文](README.zh-CN.md) · [繁體中文](README.zh-Hant.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Deutsch](README.de.md) · [Français](README.fr.md) · [Español](README.es.md)

**AI Worker**는 여러 작업에 걸쳐 식별되는 정체성, 명확한 책임, 권한 경계를 갖추고 검수 가능한 결과물을 제공하는 AI 작업 주체입니다. Teloa의 제품 명칭은 **AI 직원(AI employee)**이며, AI Worker는 그 바탕이 되는 공통 표준 개념입니다. Grok Bot도 아래 매핑 검토를 전제로 이 개념으로 설명할 수 있습니다.

## 1. 핵심 개념

| 개념 | 정의하는 내용 | 목적 |
| --- | --- | --- |
| AI Worker | 식별자, 책임, 작업 계약, 결과물 검수 조건을 갖춘 작업 주체 | 작업, 모델, 임시 Agent가 바뀌어도 작업 귀속을 추적 |
| AI Team | 공동 결과를 위한 역할 분담, 인계, 검수를 정한 구성 | 협업 결과를 계약에 따라 확인 |
| AI Organization | 조직 책임, 자원 경계, 변경 거버넌스를 갖춘 Worker와 Team의 구성 | 명시적인 거버넌스에 따라 팀이 함께 작업 |

지속적인 정체성이 전용 기기, 상주 프로세스, 장기 기억을 요구하지는 않습니다. 여기서 책임은 작업 귀속과 추적 가능성을 뜻하며 법인격을 부여하지 않습니다. 그룹 채팅이나 구성원 목록만으로 Team 계약이 성립하지 않습니다.

## 2. 범위와 형식

| 항목 | 버전 1.0 |
| --- | --- |
| 코어 객체 | `agent`, `organization` |
| 필수 작업 프로파일 | `urn:teloa:profile:ai-work`, 버전 `1.0` |
| Worker / Team / Organization | Worker: 코어 + 작업 프로파일. Team: 협업 계약 추가. Organization: 거버넌스 계약 추가 |
| 교환 형식 | JSON. YAML은 제한된 변환 규칙에 따른 선택적 작성 형식 |
| 코어 검토 항목 | 공통 규칙 8개 R01–R08, 코어 교환 사례 6개 T01–T06 |
| 해석 기준 | 중국어 간체 코어 명세와 작업 프로파일. 다른 7개 언어로 전체 읽기용 번역 제공 |

표준은 정체성, 변경 불가능한 리비전, 참조, 책임, 작업 인터페이스, 협업, 거버넌스를 정의합니다. 모델 실행, 스케줄링, 연결, 권한 부여는 호스트가 구현합니다. 정의를 가져오는 것만으로 도구 설치, 계정 연결, 작업 시작, 정기 작업 활성화가 이루어지지 않습니다. 코어 필드 6개만 있는 `agent` 문서는 완전한 AI Worker 정의가 아닙니다.

## 3. 읽기 안내

| 문서 | 내용 |
| --- | --- |
| [그림과 검색 가능한 본문으로 보는 가이드](docs/guide.ko.md) | 구성, 기술 아키텍처, 결과물 전달 흐름, 구체적인 예제 |
| [코어 명세 전문](spec/core.ko.md) | 필드, 고정 리비전, 참조, 공통 규칙, 교환 사례 |
| [작업·협업 프로파일 전문](spec/work-profile.ko.md) | 작업 계약, 자원, 인계, 정기 작업, 인스턴스 바인딩, 거버넌스 |
| [예제](examples/README.md) | 완전한 Worker JSON/YAML, 구성 예제, 입출력 |
| [제품 및 외부 명세 매핑](docs/mappings.md) | Teloa, Grok Bot, OASF, Oracle Agent Spec |
| [적합성](CONFORMANCE.md) · [유지 관리](GOVERNANCE.md) | 증거 범위, 기여 방법, 관리 |
| [8개 언어 색인](docs/languages.md) | 전체 읽기 버전과 번역 정책 |

각 가이드는 그림의 내용을 본문으로도 설명합니다. 번역본은 필드 이름, 규칙 식별자, JSON 예제를 유지합니다. 원어민 분야 전문가의 전체 검토는 아직 거치지 않았습니다. 해석이 다르면 중국어 간체 명세 두 문서를 기준으로 합니다. 공통 매핑, 예제, 관리 문서는 언어 색인에 표시된 영어 또는 중국어로 제공됩니다.

## 4. 완전한 Worker부터 시작하기

1. [구성과 결과물 전달 가이드](docs/guide.ko.md)를 읽습니다.
2. [document-worker.json](examples/document-worker.json) 또는 동등한 [YAML](examples/document-worker.yaml)을 확인합니다. `material`을 입력받고 `summary`, `openQuestions`를 반환합니다.
3. 책임, 입출력 스키마, 필수 자원, 검수 계약을 확인하고 [입력 예제](examples/document-input.json)와 [출력 예제](examples/document-output.json)를 비교합니다.
4. 실행하려면 호스트에서 정의를 실제 인스턴스, 지원되는 자원, 유효한 권한과 연결합니다. 출력, 검수, 업무 채택은 각각 기록합니다.

예제는 계약을 설명하기 위한 것으로 배포 패키지나 적합성 검증기가 아닙니다. 구성 예제에는 해결되지 않은 바인딩이 포함되어 있어 실제 실행 준비가 완료된 상태가 아닙니다.

## 5. 제품 명칭과 외부 명세

Teloa의 사용자 대상 제품 개념은 **AI 직원**입니다. Worker 정의는 역할과 작업 계약을 설명하고, Worker 인스턴스는 특정 AI 직원을 식별합니다. AI 팀은 Team 계약으로 협업과 결과물 전달을 구성합니다.

Grok의 **Bot**은 AI Worker로 매핑할 수 있습니다. 여러 사람이 공유하는 **Team Bot**은 우선 공유 Worker로 해석하며, 여러 Worker로 이루어진 팀으로 자동 간주하지 않습니다. 적합성은 이름이 아니라 정의와 호스트 동작에 따라 결정됩니다. 출처와 확인 항목은 [매핑 안내](docs/mappings.md)를 참고하세요.

OASF의 검색용 메타데이터와 분류 체계, Oracle Agent Spec의 구성 요소 참조, 인터페이스, 구성 방식을 참고했습니다. 형식이나 런타임의 완전한 호환성을 의미하지는 않습니다.

## 6. 지위, 참여, 인용

이 표준은 **Teloa가 유지 관리하는 공개 명세**이며, 채택된 국제 표준이나 인증 제도가 아닙니다. xAI, AGNTCY, Oracle 또는 표준 기관의 승인을 주장하지 않습니다. 독립적인 상호운용성에는 독립 구현의 재현 가능한 증거가 필요합니다.

구체적인 모호함, 반례, 버전을 명시한 구현 보고서는 [Issues](https://github.com/teloa-ai/ai-worker-standard/issues)에 제출해 주세요. 변경은 [유지 관리](GOVERNANCE.md), 주장 범위는 [적합성](CONFORMANCE.md)을 참고하세요.

권장 인용: **Max Luo and Morgan Chen. AI Worker Standard 1.0. Teloa, 2026.** 기계 판독용 인용 정보: [CITATION.cff](CITATION.cff).

## 7. 라이선스

이 저장소의 원본 자료는 [Apache-2.0](LICENSE) 라이선스로 제공됩니다. 제품 명칭과 외부 명세의 권리는 각 권리자에게 있습니다. 읽거나 구현하는 데 Teloa 계정이나 서비스가 필요하지 않습니다.
