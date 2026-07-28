# Skill Execution System

스킬 사용 요청부터 Target 탐색, 조건 검증, 효과 적용, 쿨타임 시작까지의 실행 흐름을 관리하는 시스템입니다.  
`PlayerSkillController`는 입력과 사용 상태를 관리하고, `SkillExecutor`는 실제 전투 판정과 효과 적용을 담당합니다.

---

## 설계 의도

스킬마다 입력 처리, 쿨타임 검사, 대상 탐색, 사거리 판정과 피해 적용을 모두 개별 구현하면 공통 조건이 반복되고 실행 흐름을 추적하기 어려워집니다.

이를 방지하기 위해 역할을 다음과 같이 분리했습니다.

- `PlayerSkillController` — 스킬 사용 요청, Runtime 조회, 사용 가능 여부 확인, 쿨타임 및 애니메이션 연결
- `SkillExecutor` — Target 탐색, 사거리 검증, SkillType별 효과 실행
- `DotStatusEffect` — 시간 기반 지속 피해 처리

핵심은 입력 시점이 아니라 **실제 스킬 실행에 성공한 경우에만 쿨타임을 시작하는 것**입니다.

---

## 주요 구현

### `PlayerSkillController` — Skill Activation Pipeline

스킬 슬롯에 대응하는 `SkillRuntime`을 조회한 뒤, 현재 스킬 사용 상태와 쿨타임을 확인합니다.

```csharp
public void TryActivateSkill(int skillIndex)
{
    SkillRuntime runtime = GetSkillRuntime(skillIndex);

    if (runtime == null || isUsingSkill)
        return;

    if (!runtime.IsReady)
        return;

    bool succeeded = ActivateSkill(runtime.SkillData);

    // 실제 실행에 성공한 경우에만 쿨타임 시작
    if (succeeded)
    {
        runtime.StartCooldown();
    }
}
```

대상이 없거나 사거리를 벗어나 `SkillExecutor`가 실패를 반환하면 Skill Effect와 쿨타임이 모두 적용되지 않습니다.

### `SkillExecutor` — 공통 실행 진입점

`TryExecute`에서 `SkillType`에 따라 실행 규칙을 분기합니다.

```text
TryExecute(skill)
├─ Normal → ExecuteNormal()
├─ Dot    → ExecuteDot()
└─ Area   → ExecuteArea()
```

### 가장 가까운 적 자동 탐색

`Physics.OverlapSphereNonAlloc`과 재사용 `Collider[]` Buffer를 사용해 주변 적을 탐색합니다.

- `EnemyHealth`가 없는 Collider 제외
- 이미 사망한 대상 제외
- `sqrMagnitude`를 사용해 가장 가까운 대상 비교
- 탐색된 대상은 별도의 `ValidateTarget`에서 Skill Range 재검증

```csharp
private EnemyHealth FindNearestTarget()
{
    int hitCount = Physics.OverlapSphereNonAlloc(
        attackOrigin.position,
        targetSearchRadius,
        hitBuffer,
        enemyLayer);

    EnemyHealth nearestTarget = null;
    float nearestDistanceSqr = float.MaxValue;

    for (int i = 0; i < hitCount; i++)
    {
        EnemyHealth candidate =
            hitBuffer[i].GetComponentInParent<EnemyHealth>();

        if (candidate == null || candidate.IsDead)
            continue;

        float distanceSqr =
            (candidate.transform.position -
             attackOrigin.position).sqrMagnitude;

        if (distanceSqr >= nearestDistanceSqr)
            continue;

        nearestDistanceSqr = distanceSqr;
        nearestTarget = candidate;
    }

    return nearestTarget;
}
```

### Target Validation

실제 스킬 효과를 적용하기 전 다음 조건을 확인합니다.

- 탐색된 대상이 존재하는가
- 대상이 사망하지 않았는가
- 대상이 해당 스킬의 사용 가능 거리 안에 있는가

거리 판정 역시 제곱 거리 상태에서 비교하여 불필요한 제곱근 계산을 피했습니다.

### Normal Skill

가장 가까운 유효 대상에게 즉시 피해를 적용합니다.

```text
FindNearestTarget()
→ ValidateTarget()
→ TakeDamage()
```

실행 성공 시 `PlayerMovement.FaceTargetInstant()`를 호출하여 플레이어가 공격 대상 방향으로 회전합니다.

### DoT Skill

단일 대상에게 최초 피해를 적용한 뒤 `DotStatusEffect`를 통해 일정 간격으로 지속 피해를 적용합니다.

```text
FindNearestTarget()
→ ValidateTarget()
→ Initial Damage
→ ApplyDot()
→ Interval-based Damage
```

동일 대상에게 DoT가 다시 적용되면 기존 Coroutine을 중단하고 새로운 DoT를 시작하는 Refresh 방식입니다.

### Area Skill

플레이어 주변의 일정 반경을 탐색하고 모든 유효 대상에게 피해를 적용합니다.

Physics 탐색은 Collider 단위로 결과를 반환하므로 하나의 Enemy가 여러 Collider를 가진 경우 중복 피해가 발생할 수 있습니다.

이를 방지하기 위해 `HashSet<EnemyHealth>`로 전투 대상 단위의 중복을 제거했습니다.

```csharp
int hitCount = Physics.OverlapSphereNonAlloc(
    areaCenter,
    skill.areaRadius,
    hitBuffer,
    enemyLayer);

uniqueTargets.Clear();

for (int i = 0; i < hitCount; i++)
{
    EnemyHealth target =
        hitBuffer[i].GetComponentInParent<EnemyHealth>();

    if (target == null || target.IsDead)
        continue;

    uniqueTargets.Add(target);
}

foreach (EnemyHealth target in uniqueTargets)
{
    target.TakeDamage(skill.damage);
}
```

---

## 구조 다이어그램

```text
Player Input / SkillButton
        │
        ▼
PlayerSkillController.TryActivateSkill()
├─ SkillRuntime 조회
├─ isUsingSkill 검사
├─ IsReady 검사
└─ ActivateSkill()
        │
        ▼
SkillExecutor.TryExecute()
├─ Normal
│    └─ Target Search → Validation → Damage
├─ DoT
│    └─ Target Search → Validation → DotStatusEffect
└─ Area
     └─ NonAlloc Search → HashSet Deduplication → Multi-target Damage
        │
        ▼
Execution Result
├─ false → Cooldown 미적용
└─ true  → StartCooldown() + Animation
```

---

## 검증 및 고려 사항

### 실행 실패 시 쿨타임 미적용

스킬 입력 직후 쿨타임을 시작하지 않고, `SkillExecutor`의 실행 결과가 `true`인 경우에만 `StartCooldown()`을 호출합니다.

이를 통해 대상 없음이나 사거리 초과와 같은 실패 요청이 플레이 상태에 영향을 주지 않도록 했습니다.

### 범위 공격의 중복 피해 방지

하나의 Enemy가 Root Collider와 Hit Collider를 동시에 보유하더라도 `HashSet<EnemyHealth>`에 한 번만 등록되기 때문에 피해는 Enemy 기준으로 한 번만 적용됩니다.

### 사망 대상 필터링

Target Search와 Area Skill 양쪽에서 `IsDead`를 검사하여 사망한 적이 새로운 공격 대상에 포함되지 않도록 했습니다.

### 현재 확장 한계

현재는 `SkillType` switch문으로 실행 규칙을 분기합니다. SkillType이 증가하면 `SkillExecutor`가 비대해질 수 있으므로, 후속 단계에서는 Targeting Rule과 Skill Effect를 독립 실행 단위로 분리하는 구조를 검토할 수 있습니다.

---

## 사용 기술

- `Physics.OverlapSphereNonAlloc` — 재사용 Buffer 기반 주변 대상 탐색
- `LayerMask` — Enemy Layer 필터링
- `Vector3.sqrMagnitude` — 제곱 거리 기반 대상 비교
- `HashSet<EnemyHealth>` — 다중 Collider 대상 중복 제거
- Coroutine — 시간 기반 DoT 처리
- Animator / Animation Event — 스킬 실행 상태와 종료 시점 연결
- `out` Parameter — 실행 결과 메시지와 Target Transform 반환
