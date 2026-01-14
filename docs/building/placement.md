## Goal 
- “입력/프리뷰/편집”은 Controller, “검증/생성”은 Manager가 담당한다.

## Key Idea
- **책임 분리:** 설치 입력/프리뷰/취소/편집 입력은 `BuildPlacementController`, 실제 검증/생성은 `BuildManager`로 위임.
- **연결형 스냅:** 통로는 “기존 노드 히트 시” 셀 단위 스냅 + 90도 회전 스냅으로 연결 UX를 안정화.
- **검증 재사용:** 이동도 `CanMoveAt → TryMoveBuilding`으로 설치 검증 파이프라인을 재사용.

## Snippet — “프리뷰 루프 + CanBuildAt 위임”
```csharp
if (isPlacing && currentArc != null && previewInstance != null)
{
    GetTargetTransform(out var pos, out var rot);

    previewInstance.transform.SetPositionAndRotation(pos, rot);

    lastCanBuild = BuildManager.I.CanBuildAt(currentArc, pos, rot, out lastError);
    previewVisual?.SetValid(lastCanBuild);

    HandleInput(pos, rot);
    return;
}
```