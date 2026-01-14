## Goal
거점 성장의 중심인 건축을 **연결형(통로/온실) + 자유배치형(시설/장치)** 모두 지원하도록 설계했습니다.
설치 과정에서 **규칙/조건 검증**을 통해 실패를 명확히 안내하고,
프리팹의 좌표/스케일/충돌 문제를 **자동 보정**해 설치 안정성을 확보했습니다.
또한 설치된 건축물은 연구·전력·녹지화 등 거점 핵심 수치와 연동되어 성장 루프를 완성합니다.

## System Overview
[![Building Flow](assets/Overview.png)](assets/Overview.png)

BuildPlacementController: “프리뷰/스냅/확정/취소/편집모드 입력”

BuildManager: “검증 → 자동보정 → 스폰 → 점유/연결 등록 → 저장/퀘스트”

BuildingInstance: “생명주기(OnEnable/Disable)에서 등록/해제 자동화”

IBuildRule: “규칙을 모듈로 분리해서 조합 가능(Flat/Collision/Limit/Attach/Cell/Occupancy…)”