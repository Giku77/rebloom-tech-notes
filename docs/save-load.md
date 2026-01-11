## Goal
크로스플랫폼 환경에서 플레이 데이터가 안정적으로 유지되도록
로컬 저장을 기본으로 하되, 로그인/네트워크 조건이 충족되면 PlayFab 서버 저장으로 자동 전환되는
하이브리드 Save/Load 구조를 설계했습니다.

## System Overview
[![Save-Load Flow](assets/save-load/Overview.png)](assets/save-load/Overview.png)