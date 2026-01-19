## Goal
- 자동화 장치(스프링클러/드론)를 “농사 로직”과 분리하고,
업그레이드 해금 시 독립 시스템을 활성화하는 방식으로 확장한다.

- 자동화는 FarmBed API만 사용해 기존 로직 수정 없이 추가되게 한다.

## System Overview

**GreenhouseUpgradeService**

- IsUnlocked/CanPurchase/Purchase로 업그레이드 구매 처리(비용 차감 + 롤백)

- Apply()에서 기능별 시스템 활성화/설정

**GreenhouseSprinklerSystem**

- 주기적으로 FarmBed.CanWater 검사 → FarmBed.Water 호출

**GreenhouseFarmDroneSystem**

- 자동 수확/자동 비료 같은 기능을 토글로 확장 (SetAutoFertilize(true))

## Snippet — Apply의 의미
```csharp
case 2: // 스프링클러 설치
    ctx.GetComponentInChildren<GreenhouseSprinklerSystem>(true)?.gameObject.SetActive(true);
    break;

case 4: // 농사용 드론 설치
    ctx.GetComponentInChildren<GreenhouseFarmDroneSystem>(true)?.gameObject.SetActive(true);
    break;

case 5: // 드론 강화(자동 비료)
    ctx.GetComponentInChildren<GreenhouseFarmDroneSystem>(true)?.SetAutoFertilize(true);
    break;
```    

- 업그레이드 로직은 “구매/해금/적용”만 담당

- 실제 자동화 행동은 각 시스템 클래스가 담당
**→ 기능 추가 = 클래스 추가 + Apply switch 확장(또는 나중엔 function→handler 맵으로 더 깔끔하게)**