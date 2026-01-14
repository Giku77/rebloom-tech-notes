
## Goal
footprint 기반 셀 점유로 **“겹침/중복 설치”**를 시스템적으로 차단한다.

## Key Idea
- **footprint → 셀 영역 계산 → `GridOccupancyManager.Occupy`** 흐름으로 “이미 점유된 공간”에 대한 설치를 원천 차단한다.
- 이동/삭제 시에도 **Release → Occupy 재적용**으로 점유 상태를 항상 일관되게 유지한다.

## Snippet A — footprint → cells
```csharp
Vector2Int baseCell = CorridorGrid.WorldToCell(pos);
int hx = sx / 2, hz = sz / 2;

for (int x = -hx; x < -hx + sx; x++)
    for (int z = -hz; z < -hz + sz; z++)
        yield return new Vector2Int(baseCell.x + x, baseCell.y + z);
```

## Snippet B — Spawn()에서 점유 등록
```csharp
var fp = footprintProvider.GetFootprint(arc);
var cells = GetCellsFromFootprint(fp, p.transform.position, p.transform.rotation);

GridOccupancyManager.I?.Occupy(bInstance, cells);
AutoSaveService.I?.RequestSave("Build");
```
