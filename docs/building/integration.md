## Goal
건축을 “단독 기능”이 아니라 성장 루프(재료/퀘스트/세이브)와 연결한다.

## Key Idea
- 레시피 기반 재료 검증/소모로 설치 UX를 일관되게 유지한다.
- 설치 완료 시 `QuestManager`에 통지해 퀘스트 진행과 즉시 연결한다.
- 설치/이동/삭제 시 `AutoSaveService.RequestSave()`로 진행 보존을 자동화한다.

## Snippet A — TryBuild 재료 체크/소모
```csharp
if (!recipeDB.TryGetRecipe(arcId, out var recipe))
    return Spawn(arc, pos, rot);

if (!debugBuildingMode)
{
    if (!HasMaterials(recipe))
    {
        ToastMessageUI.Instance.Show("재료가 부족합니다.");
        return false;
    }
    Remove(recipe);
}

return Spawn(arc, pos, rot);
```

## Snippet B — Spawn 마무리(퀘스트 + 저장)
```csharp
QuestManager.I?.NotifyBuildingBuilt(arc.arcId);
AutoSaveService.I?.RequestSave("Build");
return true;
```