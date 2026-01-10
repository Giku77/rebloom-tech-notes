# Farming Automation & Building Integration

## Goal
플레이어의 반복 행동(물주기/운반)을 줄이고, 거점 확장에 따라 농사가 **자동 생산 라인**으로 성장하도록 설계.

## System Overview
**WaterTank → Sprinkler → Drone → FarmStorage** 파이프라인으로
물 공급–관리–회수–보관이 끊기지 않게 연결.

```mermaid
flowchart LR
  A[WaterTank\n물 공급] --> B[Sprinkler\n자동 물주기]
  B --> C[Farm Plot\n성장/상태]
  C --> D[Drone\n수확물 회수]
  D --> E[FarmStorage\n자동 수납]
