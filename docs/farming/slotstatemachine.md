## Goal
Empty → Growing → Mature → Empty

**전환 트리거**

- Plant: Empty → Growing

- Stage complete: stageIndex++ (마지막이면 Mature)

- Harvest/Uproot: Mature/Growing → Empty

## Key Idea
- 물 요구량 미달이면 시간 진행 중단

- 비료는 duration 동안 multiplier 적용 후 자동 원복

## Snippet — TickSlot (상태 전이의 핵심)
```csharp
if (slot.state != CropSlotState.Growing) return;

var stage = row.stages[slot.stageIndex];

// 물 부족이면 성장 정지
if (stage.needWater > 0 && slot.wateredCount < stage.needWater)
    return;

// 비료 버프 지속시간 처리
if (slot.fertilizerRemain > 0f) {
    slot.fertilizerRemain -= dt;
    if (slot.fertilizerRemain <= 0f) {
        slot.fertilizerRemain = 0f;
        slot.growSpeedMultiplier = 1f;
    }
}

slot.stageTimer += dt * Mathf.Max(1f, slot.growSpeedMultiplier);

if (slot.stageTimer >= stage.needTime) {
    slot.stageTimer = 0f;
    slot.wateredCount = 0;
    slot.stageIndex++;

    if (slot.stageIndex >= row.stages.Length - 1)
        slot.state = CropSlotState.Mature;

    UpdateSlotVisual(index);
    OnChanged?.Invoke();
}
```

## Why this matters

“성장 규칙”이 Tick 안에 고정되어 있어도, 실제 값(needTime/needWater/prefabKey)은 FarmDB로 교체 가능해서 콘텐츠 확장이 쉬움.

## Visual Loading (Addressables + Cancel)

성장 단계가 바뀌면 prefabKey가 바뀌기 때문에, 로딩 경쟁이 쉽게 생김 → 슬롯별 CTS로 이전 로딩 취소.

## Snippet — UpdateSlotVisualAsync(핵심)
```csharp
_slotCts[index]?.Cancel();
_slotCts[index]?.Dispose();
_slotCts[index] = new CancellationTokenSource();
var token = _slotCts[index].Token;

if (slot.visualRoot != null) {
    FarmPrefabProvider.I.ReleaseAddressableInstance(slot.visualRoot);
    slot.visualRoot = null;
    slot.visual = null;
}

var go = await FarmPrefabProvider.I.InstantiateAddressableAsync(prefabKey, ...);
if (token.IsCancellationRequested) {
    if (go != null) FarmPrefabProvider.I.ReleaseAddressableInstance(go);
    return;
}
slot.visualRoot = go;
slot.visual = go.GetComponentInChildren<CropVisual>(true);
```
**“단순 로딩”이 아니라 스테이지 전환/취소/정리(ReleaseInstance) 까지 포함해 메모리 누수/중복 인스턴스/경합을 방지했다.**

## External API (Player/Automation 공용)

- CanPlant/Plant

- CanWater/Water, TryWaterByPlayer(인벤 소모 포함)

- TryApplyFertilizer, TryFertilizeByPlayer

- CanHarvest/Harvest (+ 내부용 TryHarvestInternal)

**자동화 시스템은 여기 API만 호출하면 되고, FarmBed 내부 구현을 몰라도 됨 → 확장 포인트**

## Performance Note

현재는 FarmBed.Update()에서 슬롯 8개 Tick.

- 장점: 구현 간단, 디버그 쉬움

- 확장 시: FarmBed가 많아지면 Update 분산 호출 비용 증가 가능

**개선 방향:**

- FarmingManager에서 모든 FarmBed를 등록/관리하고,
일정 주기(예: 0.2s)로 Tick 하거나, 활성 Bed만 Tick 하는 구조로 확장 가능