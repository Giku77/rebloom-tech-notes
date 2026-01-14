## Goal
규칙을 모듈로 조합하고, 실패 원인을 표준화해 UI 안내가 명확해진다.

## Key Idea
- `IBuildRule` 리스트로 규칙을 조합해 **확장 가능**(추가/삭제).
- `Validate(ctx, out errorCode)`로 실패 사유를 **한 채널로 표준화** → Toast/UI 메시지 일관.

## Evidence (Code)

### Snippet A — InitRules()
```csharp
private void InitRules()
{
    buildRules.Add(new FlatSurfaceRule(buildableLayer, maxHeightDiff, maxSlopeAngle));
    buildRules.Add(new CollisionRule(obstacleLayer));
    buildRules.Add(new LimitRule(this));
    buildRules.Add(new CorridorAttachRule());
    buildRules.Add(new CorridorCellRule());
    buildRules.Add(new OccupancyRule());
}
```

### Snippet B — Validate()
```csharp
public bool Validate(ArcContext ctx, out string errorCode)
{
    foreach (var rule in buildRules)
        if (!rule.Validate(ctx, out errorCode))
            return false;

    errorCode = null;
    return true;
}

