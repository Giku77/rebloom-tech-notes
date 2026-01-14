## Goal
`CorridorNode`의 **Cell 기반**으로 “붙이기/연결”을 **예측 가능**하게 만든다.

## Key Idea
- **히트한 통로의 `Cell` + 맞춘 면 방향**으로 `targetCell`을 계산해, “어디에 붙는지”가 항상 동일 규칙으로 결정된다.
- `CellToWorldCenter()`로 **셀 중심 스냅**을 적용해 자연스러운 배치 UX를 만든다.
- 소켓/연결 매니저(`CorridorSocketManager` 등)로 **연결 상태를 시스템적으로 관리**한다.

## Snippet A — 기존 통로 히트 → targetCell 계산
```csharp
Vector2Int baseCell = hitNode.Cell;

Vector3 localHit = hitNode.transform.InverseTransformPoint(hit.point);
CorridorDirection localDir =
    (Mathf.Abs(localHit.x) > Mathf.Abs(localHit.z))
    ? (localHit.x > 0 ? CorridorDirection.East : CorridorDirection.West)
    : (localHit.z > 0 ? CorridorDirection.North : CorridorDirection.South);

CorridorDirection worldDir =
    CorridorConnectionManager.RotateDirection(localDir, baseRotIndex);

Vector2Int targetCell =
    baseCell + CorridorConnectionManager.DirectionToOffset(worldDir);

pos = CorridorGrid.CellToWorldCenter(targetCell, hitNode.transform.position.y);
rot = hitNode.transform.rotation; 
```

## Snippet B — RegisterBuilding에서 소켓 등록
```csharp
var sp = inst.GetComponent<CorridorSocketProvider>();
if (sp != null)
{
    int rotIndex = CorridorGrid.GetRotIndex(inst.transform.rotation);
    Vector2Int baseCell = CorridorGrid.WorldToCell(inst.transform.position);
    CorridorSocketManager.I?.RegisterSockets(sp, baseCell, rotIndex);
}
```