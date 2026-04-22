# Results

## 1. 실험 요약
- 저장소: bench-model-load-and-cache
- 커밋 해시: d7fe1e9
- 실험 일시: 2026-04-22T06:14:38.712Z -> 2026-04-22T06:14:39.117Z
- 담당자: ai-webgpu-lab
- 실험 유형: `benchmark`
- 상태: `success`

## 2. 질문
- cold load와 warm load의 total/init delta가 cache state와 함께 재현되는가
- prepared artifact hit 여부가 raw JSON과 결과 문서에서 같이 보이는가
- 실제 model/runtime 교체 전 cache benchmark 프로토콜을 고정할 수 있는가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 16 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: not-applicable
- backend: `mixed`
- fallback triggered: `false`
- worker mode: `main`
- cache state: `cold, warm`
- required features: []
- limits snapshot: {}

## 4. 워크로드 정의
- 시나리오 이름: Cold Load, Warm Load
- 입력 프로필: 32768-synthetic-tokens
- 데이터 크기: manifestFetchMs=21.3; materializeMs=11.2; cacheReadMs=0; prepareMs=4; preparedHit=false; Run both scenarios to capture cold/warm delta.; automation=playwright-chromium, manifestFetchMs=11.8; materializeMs=0; cacheReadMs=3.3; prepareMs=0; preparedHit=true; coldTotalMs=45.2; warmTotalMs=15.1; deltaMs=30.1; automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: synthetic-browser-load-v1
- 양자화/정밀도: -
- resolution: -
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 170 ~ 575.9 ms
- init_ms: 15.1 ~ 45.2 ms
- success_rate: 1
- peak_memory_note: 16 GB reported by browser
- error_type: -

### LLM / Benchmark
- init_ms: 15.1 ~ 45.2 ms
- cache states: cold, warm
- prepared hit states: false, true

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Cold Load | mixed | cold | 45.2 | - | cache=cold, preparedHit=false |
| 2 | Warm Load | mixed | warm | 15.1 | - | cache=warm, preparedHit=true |

## 7. 관찰
- cold init_ms=45.2 ms, warm init_ms=15.1 ms, delta=30.1 ms였다.
- warm run meta.notes에는 preparedHit=true가 남아 cache reuse 경로가 실제로 기록됐다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. 결론
- cold/warm load delta가 처음으로 raw JSON과 RESULTS.md 둘 다에 기록됐다.
- 다음 단계는 실제 model asset과 cache eviction 조건을 추가해 warm hit/miss 경계를 더 분명히 하는 것이다.
- 브라우저별 storage path 차이와 fallback mode를 별도 결과로 누적해야 한다.

## 9. 첨부
- 스크린샷: ./reports/screenshots/01-cold-load.png, ./reports/screenshots/02-warm-load.png
- 로그 파일: ./reports/logs/01-cold-load.log, ./reports/logs/02-warm-load.log
- raw json: ./reports/raw/01-cold-load.json, ./reports/raw/02-warm-load.json
- 배포 URL: https://ai-webgpu-lab.github.io/bench-model-load-and-cache/
- 관련 이슈/PR: -
