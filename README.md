# 황다연 | 신한DS 정보보안 포트폴리오

개인정보 동의항목과 위험도 계산을 API로 구현하고 테스트로 검증했습니다. 금융보안 프로젝트에서는 조건별 판단 근거와 정보 누락을 구분하는 기능을 맡았습니다.

| 프로젝트 | 개인 담당 | 확인 근거 |
|---|---|---|
| 동의ON | 위험도 계산, 동의항목 조회 API, 회귀 테스트 | 작성 PR 2건, 공용 모델 28개와 조회 서비스 3개 테스트 통과 기록 |
| nodown | 거래 탐지, 상태 모의조회, 기본요건 규칙 엔진 | 담당 지침, 규칙 구현과 테스트 |
| LogosFuzz | C/C++ API 추출, 빌드 정보와 KB 연결 | 작성 PR 2건, libsndfile 환경 검증 기록 |

## 동의ON: 동의항목과 사용자 선택을 확인하는 API

개인정보처리방침을 구조화해 위험도를 보여주고 동의 변경을 지원하는 팀 프로젝트입니다. 위험도 계산과 백엔드 흐름을 맡았습니다.

기업별 필수 및 선택 동의항목 조회 API를 구현했습니다. 사용자의 실제 선택 여부와 민감도, 외부 제공, 보유기간, 처리 목적, AI 활용 관련 값을 함께 반환하도록 구성했습니다. 공용 모델에는 산출식, 등급 경계, LLM 응답 파싱과 저장 변환 테스트를 추가했습니다. 숫자 저장 형식과 등급 매핑의 차이를 확인해 통합 가이드에 기록했습니다.

- [회귀 테스트와 타입 변환 PR #20](https://github.com/dayeon053/dynamic-consent-system/pull/20)
- [동의항목 조회 API PR #21](https://github.com/dayeon053/dynamic-consent-system/pull/21)
- [원본 개발 브랜치](https://github.com/dayeon053/dynamic-consent-system/tree/develop)

두 PR은 develop에 병합됐습니다. 공용 모델 28개와 조회 서비스 3개 테스트 통과 기록이 있으며, 별도 backend contextLoads 기존 실패가 남아 있었습니다. 테스트 결과는 각 PR의 기록 기준입니다. 프로젝트의 위험도 모델과 API 동작을 검증했으며 개인정보보호 법규 준수 여부를 인증한 결과는 아닙니다.

## nodown: 판단 조건과 정보 누락을 구분하는 금융 서비스

장기할부 서비스 중단 시 피해 가능 거래를 선별하고 할부항변 준비를 돕는 팀 프로젝트입니다. 거래 탐지, 가맹점 상태 모의조회, 사례 API와 기본요건 규칙 엔진을 맡았습니다.

같은 입력에 같은 판정과 근거가 나오도록 조건별 규칙을 적용했습니다. 자료가 빠진 경우에는 추가 확인 상태를 반환하고, 조건을 충족하지 못하는 거래에는 제외 사유를 표시했습니다. 결과 화면에서 조건의 충족 여부와 판단 근거를 확인하도록 구성했습니다.

- [원본 저장소](https://github.com/10502R/nodown)
- [규칙 엔진](https://github.com/10502R/nodown/blob/main/services/rule_service.py)
- [규칙 테스트](https://github.com/10502R/nodown/blob/main/tests/test_rule_service.py)
- [담당 범위와 완료 기준](https://github.com/10502R/nodown/blob/main/docs/A_%EB%8B%B4%EB%8B%B9_%EA%B5%AC%ED%98%84%EC%A7%80%EC%B9%A8.md)

합성 거래와 모의조회를 사용한 프로토타입입니다. 실제 카드사 및 국세청 연계와 권리 인정 판단은 검증 범위에 포함하지 않았으며 최종 판단 주체를 화면에 표시했습니다.

## LogosFuzz: 분석 결과의 실행 조건을 함께 관리

LLM의 퍼징 하네스 생성에 API 선언과 빌드 조건을 제공하는 팀 프로젝트입니다. Clang AST 기반 API 분석, 빌드 단위별 지식베이스 구성과 생성기 연결을 맡았습니다.

대상 프로필과 compile_commands.json에서 파일별 컴파일 옵션을 읽는 기능을 구현했습니다. 비-Bazel 빌드에서도 KB를 하네스 계획에 연결하도록 확장했고, 평가용 정답셋 생성에서 컴파일 플래그가 누락되는 문제를 수정했습니다.

- [원본 개발 브랜치](https://github.com/sesezz/LogosFuzz/tree/dev)
- [프로필과 추출기 PR #71](https://github.com/sesezz/LogosFuzz/pull/71)
- [KB 연결과 평가 PR #76](https://github.com/sesezz/LogosFuzz/pull/76)
- [실행 및 평가 문서](https://github.com/sesezz/LogosFuzz/blob/feature/generic-embedded/docs/GENERIC-A-kb.md)

WSL libsndfile 1.2.2에서 92개 파일의 컴파일 정보를 추출하고 최소 하네스 200회 실행을 확인했습니다. PR #76의 전체 테스트 기록은 1142개 통과, 5개 건너뛰기, 기존 실패 6개입니다. 선정 API 8개와 해당 환경에서의 결과이며 전체 라이브러리 정확도나 기존 퍼저 대비 성능 우위를 검증한 수치는 아닙니다.
