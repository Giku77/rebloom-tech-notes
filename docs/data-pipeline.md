## Goal
Re:Bloom은 아이템·퀘스트·건축·농사·스탯 등 시스템이 많고, 밸런스/콘텐츠가 빠르게 바뀌는 프로젝트입니다.
이를 코드 수정 없이 확장·운영할 수 있도록 BG Database 기반의 데이터 테이블 파이프라인을 구축했습니다.

- 기획/운영 데이터는 테이블에서 관리
- 런타임에 테이블을 로딩해 시스템에 주입
- 시스템은 “데이터를 읽어 동작”하도록 설계하여 콘텐츠 확장 비용을 최소화

## System Overview
[![Data-PipeLine](assets/data-pipeline/Overview.png)](assets/data-pipeline/Overview.png)