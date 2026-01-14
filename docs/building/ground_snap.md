# Ground Snap (Auto Adjust)

## Goal
“바닥/경사/파묻힘” 문제를 자동 보정해 설치 안정성을 확보한다.

## Key Idea
- **footprint 4코너 + center 레이캐스트**로 “실제 지면”을 샘플링한다.
- **maxHeightDiff**로 경사/단차를 제한해 설치 불가 조건을 명확히 만든다.
- **DepthOffset**으로 프리팹별 파묻힘/뜸을 보정해 최종 Y를 안정화한다.

## Snippet — TryAdjustToGround (핵심)
```csharp
float minY = float.MaxValue, maxY = float.MinValue;
int hitCount = 0;

for (int i = 0; i < localOffsets.Length; i++)
{
    Vector3 worldPos = center + rot * localOffsets[i] + Vector3.up * 5f;
    if (Physics.Raycast(worldPos, Vector3.down, out var hit, 20f, buildableLayer))
    {
        hitCount++;
        minY = Mathf.Min(minY, hit.point.y);
        maxY = Mathf.Max(maxY, hit.point.y);
    }
}

if (hitCount == 0) { errorCode = "NO_GROUND"; return false; }
if ((maxY - minY) > maxHeightDiff) { errorCode = "SLOPE_TOO_HIGH"; return false; }

adjustedPos = new Vector3(center.x, maxY - ctx.DepthOffset, center.z);
```