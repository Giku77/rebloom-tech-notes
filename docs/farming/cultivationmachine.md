## Goal
- FarmBed(다슬롯)와 다른 타입의 “농사 생산 설비”를 단일 슬롯 상태머신으로 구현

- 시작 조건(전력/아이템 유효성)과 결과 수거를 명확히 분리

## Key Idea

- CanStart()에서 실패 사유(reason) 제공

- Tick()은 Running 상태 + 전력 조건에서만 시간 감소

- 완료 시 ReadyToCollect → Collect로 인벤 반영 후 슬롯 초기화