# Room Manager

플레이어의 시점 전환, 확대 화면, UI 버튼 상태를 통합 관리하는 핵심 매니저입니다.

---

## 설계 의도

방 안에는 여러 상태가 동시에 존재합니다. 오브젝트 조사 중일 수도 있고, 화면이 확대된 상태일 수도 있고, 대화가 진행 중일 수도 있습니다. 처음에는 각 상태가 변할 때마다 버튼을 개별적으로 켜고 껐는데, 상태가 늘어날수록 버튼이 엉뚱하게 표시되는 경우가 자주 생겼습니다.

`SetButtons()` 하나에 모든 상태 플래그를 읽어 버튼 표시를 한 번에 결정하는 방식으로 바꿨습니다. 어떤 상태가 변하든 `SetButtons()`만 호출하면 항상 올바른 상태가 됩니다.

---

## 주요 구현

**시점 전환 (`MoveSides`)**

- `sides` 리스트에서 현재 인덱스를 기준으로 좌/우 이동합니다. 모듈러 연산(`% sides.Count`)으로 끝에서 처음으로 순환합니다.
- `SetCurrentView`는 이전 뷰를 `SetActive(false)`, 새 뷰를 `SetActive(true)`로 전환합니다. 씬 초기화 시 모든 Side를 한 번씩 켰다 끄는 것은 각 오브젝트의 `Awake`/`Start`를 정상적으로 실행시키기 위함입니다.
- 튜토리얼 진행 중이면 `SetSeenSide(newSideIndex)`로 튜토리얼 매니저에 시점 변경을 알립니다.

**UI 버튼 통합 관리 (`SetButtons`)**

모든 상태 플래그를 한 자리에서 읽습니다.

```csharp
bool isInvestigatingOrZoomed = isInvestigating || isZoomed;
bool isDialogueActive        = DialogueManager.Instance.isDialogueActive;
bool isMemoOpen              = MemoManager.Instance.isMemoOpen;
bool isLaptopOpen            = (bool)GameManager.Instance.GetVariable("isLaptopOpen");
```

이 값들을 조합해 나가기 버튼과 이동 버튼의 표시 여부를 결정합니다. 상태가 추가되더라도 이 함수 하나만 수정하면 됩니다.

**무한 입력 방지 (`ProhibitInput`)**

`heartParent.transform.childCount < 1`, 즉 하트가 모두 소진된 상태에서 추가 클릭이 들어오면 `OnExitButtonClick()`을 강제로 호출해 현재 조사/확대 상태를 해제합니다. 퍼즐이나 잠금 오브젝트에서 행동력 없이 무한 입력을 시도하는 상황을 차단합니다.

**날짜 전환 지연 실행**

조사 또는 대화 중 행동력이 소진되면 즉시 날짜 전환 연출을 실행하지 않고, `RefillHeartsOrEndDay` 게임 변수를 `true`로 설정해 예약합니다. `OnExitButtonClick`에서 조사/대화가 종료된 뒤 이 변수를 확인하고 날짜 전환을 실행합니다.

---

## 구조 다이어그램

```
RoomManager
├─ MoveSides(leftOrRight)
│     ├─ SetCurrentSide(newIndex) → SetCurrentView()
│     ├─ UIManager.MoveSideEffect()
│     └─ TutorialManager.SetSeenSide()
├─ OnExitButtonClick()
│     ├─ 조사 중 → imageAndLockPanelManager.OnExitButtonClick()
│     ├─ 확대 중 → SetCurrentView(side), isZoomed = false
│     └─ RefillHeartsOrEndDay 변수 확인 → 날짜 전환 지연 실행
├─ SetButtons()
│     → isInvestigating, isZoomed, isDialogueActive,
│       isMemoOpen, isLaptopOpen 조합으로 버튼 상태 결정
└─ ProhibitInput()
      → childCount < 1이면 OnExitButtonClick() 강제 호출
```

---

## 트러블슈팅

**조사/확대 중 이동 버튼 표시 문제**

상태가 변할 때마다 각자 버튼을 건드리다 보니 특정 상태 조합에서 버튼이 잘못 표시되는 경우가 많았습니다. 예를 들어 확대 상태에서 대화가 시작되면 이동 버튼이 다시 나타나는 식이었습니다. `SetButtons()` 하나로 통합한 뒤에는 어떤 상태 조합이든 일관되게 처리됩니다.

**씬 초기화 시 오브젝트 `Awake` 미실행 문제**

Start에서 모든 Side를 `SetActive(false)`로 끄면, 처음부터 비활성인 오브젝트들은 `Awake`가 실행되지 않아 초기화가 누락됩니다. 모든 Side를 한 번씩 켰다 끄는(`SetActive(true)` → `SetActive(false)`) 방식으로 해결했습니다.

```csharp
foreach (GameObject side in sides)
{
    side.SetActive(true);   // Awake 실행
    side.SetActive(false);  // 비활성으로 복귀
}
```

---

## 사용 기술

- 다중 상태 플래그 통합 관리 (`SetButtons` 단일 진입점)
- `SetActive` 활용한 씬 초기화 패턴
- `GameManager` 변수 기반 상태 예약 (날짜 전환 지연 실행)
- 싱글턴 패턴 (`Instance`)
