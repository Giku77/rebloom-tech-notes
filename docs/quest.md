## Goal
튜토리얼부터 메인 퀘스트, 최종 엔딩 컷씬까지 플레이 경험이 끊기지 않도록
퀘스트 진행을 **데이터 기반**(조건 타입/목적지/보상)으로 설계하고,
각 조건을 **상호작용 시스템과 연결**해 확장 가능한 구조로 구현했습니다.

## System Overview
```mermaid
flowchart LR
  A[Tutorial Complete] --> B[Main Quest Start]
  B --> C{Condition Type}
  C -->|Interact| D[Interaction Binding]
  C -->|Reach Destination| E[Waypoint Guide]
  D --> F[Progress Update]
  E --> F
  F --> G[UI Feedback / Toast]
  F --> H{Quest Finished?}
  H -->|No| B
  H -->|Yes| I[Reward & Next Quest]
  I --> J[Ending Cutscene]

