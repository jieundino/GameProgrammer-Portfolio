# Hey Cheese! Mini-game Interaction System

Unity 기반 Android 교육용 인터랙티브 콘텐츠 **"헤이 치즈!"**에서 구현한
**에피소드형 미니게임 진행 흐름, 터치 기반 오브젝트 상호작용, 상태 기반 피드백 구조**를 정리한 포트폴리오입니다.

본 문서는 전체 프로젝트 중 제가 담당한 **미니게임 클라이언트 구현 코드**를 중심으로 작성되었습니다.

<br>

## 📌 Project Overview

**"헤이 치즈!"**는 경계선 지능 아동의 감정 표현과 사회적 관계 형성을 지원하기 위해 개발한
Unity 기반 Android 교육용 인터랙티브 콘텐츠입니다.

사용자는 캐릭터와 함께 에피소드를 진행하며, 선택지, 미니게임, 감정 표현, 긍정적 피드백을 통해
자연스럽게 사회적 상황과 감정 표현을 경험합니다.

<br>

| 항목        | 내용                                |
| :-------- | :-------------------------------- |
| **프로젝트명** | 헤이 치즈! (Hey Cheese!)              |
| **장르**    | 교육용 인터랙티브 콘텐츠, 기능성 게임             |
| **플랫폼**   | Android                           |
| **엔진**    | Unity                             |
| **개발 언어** | C#                                |
| **개발 기간** | 2025.03 ~ 2025.07                 |
| **팀 구성**  | 4명                                |
| **담당 역할** | Mini-game Client Programmer       |
| **주요 성과** | 사용자 테스트용 빌드 제작, JCCT 학술논문 제2저자 게재 |

<br>

## 🙋‍♀️ My Role

본 프로젝트에서 저는 **에피소드 내 미니게임 클라이언트 구현**을 담당했습니다.

주요 역할은 다음과 같습니다.

* 에피소드별 미니게임 진행 로직 구현
* 터치 기반 오브젝트 선택 및 상호작용 처리
* 미니게임별 완료 조건 및 재시작 흐름 구현
* Slider, TextMeshPro, Sprite 변경을 활용한 상태 피드백 구현
* Coroutine 기반 제한 시간, 카운트다운, 이동 연출 처리
* 사용자 테스트용 Android 빌드 제작 및 주요 기능 동작 점검

<br>

## 🛠️ Tech Stack

| 분류                       | 사용 기술                                                   |
| :----------------------- | :------------------------------------------------------ |
| **Engine**               | Unity                                                   |
| **Language**             | C#                                                      |
| **UI**                   | Unity UI, TextMeshPro, Slider, Button, Image            |
| **Input**                | Touch Input, Mouse Input for Editor Test                |
| **Logic**                | Coroutine, Dictionary, Enum, List                       |
| **Animation / Feedback** | Animator, Sprite Change, Lerp Movement, UI State Change |
| **Platform**             | Android                                                 |

<br>

## 📂 Code Structure

```text
HeyCheese
├─ MiniGameManager.cs
├─ MiniGame1Manager.cs
├─ HiddenCharacterButton.cs
├─ MiniGame2_1Manager.cs
├─ FoodButton.cs
├─ CharacterReaction.cs
├─ MiniGame2_2Manager.cs
├─ DirtyDishButton.cs
├─ TrashButton.cs
├─ MiniGame3Manager.cs
├─ PlayerController.cs
├─ ScrollingBackground.cs
├─ MiniGame4Manager.cs
├─ TouchIndicatorController.cs
└─ AlphaImage.cs
```

<br>

## 🧩 Implemented Systems

## 1. Common Mini-game Flow

### 핵심 코드

* `MiniGameManager.cs`

### 구현 목적

여러 미니게임에서 공통적으로 필요한 **가이드 패널, 클리어 패널, 게임 시작, 메인 스토리 복귀 흐름**을
공통 기반 클래스로 분리했습니다.

### 주요 구현

* `abstract class MiniGameManager` 기반 공통 구조 설계
* `StartGame()` 추상 메서드를 통해 미니게임별 시작 로직 분리
* `GuidePanel`, `ClearPanel` 공통 제어
* 미니게임 종료 후 메인 스토리 씬으로 복귀하는 Coroutine 구현

```csharp
abstract public class MiniGameManager : MonoBehaviour
{
    [SerializeField] protected GameObject GuidePanel;
    [SerializeField] protected GameObject ClearPanel;

    protected void Awake()
    {
        GuidePanel.SetActive(true);
        ClearPanel.SetActive(false);
    }

    public abstract void StartGame();

    public IEnumerator BackToMainStory()
    {
        yield return new WaitForSecondsRealtime(2f);

        MainStoryGameManager.MainStoryGM.NextStep();
        UnityEngine.SceneManagement.SceneManager.LoadScene("MainStory");
        Time.timeScale = 1f;
    }
}
```

### 개선 포인트

미니게임마다 공통으로 반복되는 시작, 종료, 복귀 흐름을 상위 클래스로 분리하여
각 미니게임 매니저에서는 개별 규칙과 진행 조건에 집중할 수 있도록 구성했습니다.

<br>

---

<br>

## 2. Hide-and-Seek Mini-game

### 핵심 코드

* `MiniGame1Manager.cs`
* `HiddenCharacterButton.cs`

### 구현 목적

사용자가 제한 시간 안에 숨겨진 캐릭터를 찾아 터치하는 방식의 미니게임입니다.
아동 사용자가 쉽게 이해할 수 있도록 **짧은 제한 시간, 명확한 정답 표시, 단계별 진행 구조**를 중심으로 구현했습니다.

### 주요 구현

* 제한 시간 기반 타이머 Slider 구현
* 스테이지별 숨은 캐릭터 오브젝트 활성화
* 캐릭터 클릭 시 정답 원 표시 및 버튼 비활성화
* 찾은 캐릭터 수에 따른 클리어 조건 판정
* 실패 시 재시작 패널 표시
* 스테이지별 가이드 문구 변경
* 캐릭터 아이콘 Alpha 값을 활용한 진행 상태 표시

### Flow

```text
Guide Panel
    ↓
StartGame()
    ↓
Stage 활성화
    ↓
Timer 시작
    ↓
숨은 캐릭터 터치
    ↓
정답 표시 + Score 증가
    ↓
모든 캐릭터 발견 여부 검사
    ↓
Clear Panel 또는 Restart Panel
```

### 관련 코드 요약

```csharp
public void onClick()
{
    SoundPlayer.Instance.SoundEffectPlay((int)SoundPlayer.SFX.Correct_SFX);

    CorrectCircleImage.SetActive(true);
    characterButton.interactable = false;

    miniGame1Manager.IncrementFindOutScore();
    miniGame1Manager.SetCharacterIcon((int)characterName);
}
```

### 개선 포인트

단순히 캐릭터를 클릭하는 구조가 아니라,
**정답 표시, 아이콘 변화, 제한 시간, 재시작 흐름**을 함께 연결하여
사용자가 현재 진행 상태를 쉽게 파악할 수 있도록 구현했습니다.

<br>

---

<br>

## 3. Food Selection Mini-game

### 핵심 코드

* `MiniGame2_1Manager.cs`
* `FoodButton.cs`
* `CharacterReaction.cs`

### 구현 목적

사용자가 음식을 선택하면 접시에 음식이 추가되고,
선택 상태에 따라 캐릭터의 표정이 변화하는 미니게임입니다.

### 주요 구현

* `Dictionary<int, bool>` 기반 음식 선택 상태 관리
* 음식 버튼 클릭 시 선택 상태 갱신
* 선택한 음식에 대응하는 접시 이미지 활성화
* 선택된 음식 개수에 따라 캐릭터 반응 Sprite 변경
* 모든 음식 선택 완료 시 Clear Panel 표시

### Flow

```text
StartGame()
    ↓
음식 버튼 터치
    ↓
FoodState Dictionary 갱신
    ↓
접시 이미지 활성화
    ↓
선택 개수 계산
    ↓
캐릭터 표정 변경
    ↓
모든 음식 선택 시 클리어
```

### 관련 코드 요약

```csharp
public void SetFoodStateDictionary(FoodName foodName, bool isTrue)
{
    foodState[(int)foodName] = isTrue;
    PutFooldInPlate(foodName);
}
```

```csharp
public void ChangeReactionImage()
{
    reactionState = (ReactionState)miniGame2_1Manager.GetFoodInPlateNum();
    characterImage.sprite = ReactionImages[(int)reactionState];
}
```

### 개선 포인트

음식 선택 상태를 개별 Boolean 변수로 관리하지 않고 Dictionary로 관리하여,
선택 여부 확인과 완료 조건 판정을 간단하게 처리했습니다.

또한 사용자의 선택이 캐릭터의 표정 변화로 즉시 연결되도록 하여
아동 사용자가 자신의 행동 결과를 직관적으로 이해할 수 있도록 구현했습니다.

<br>

---

<br>

## 4. Clean-up Mini-game

### 핵심 코드

* `MiniGame2_2Manager.cs`
* `DirtyDishButton.cs`
* `TrashButton.cs`

### 구현 목적

사용자가 흩어진 물건을 정리하고, 할 일 목록의 진행도를 채우는 미니게임입니다.
각 할 일마다 완료 기준이 다르기 때문에 **항목별 진행도 카운트와 완료 조건 판정**을 분리하여 구현했습니다.

### 주요 구현

* `Dictionary<int, bool>` 기반 할 일 완료 상태 관리
* `List<int>` 기반 현재 진행 개수와 완료 기준 관리
* TextMeshProUGUI를 활용한 할 일 진행도 표시
* 완료된 항목에 취소선 표시
* 오브젝트 클릭 시 비활성화 또는 위치 이동 처리
* 모든 할 일 완료 시 Glitter Effect 및 Clear Panel 표시

### Flow

```text
StartGame()
    ↓
할 일 목록 초기화
    ↓
오브젝트 터치
    ↓
항목별 Count 증가
    ↓
TMP 진행도 업데이트
    ↓
완료 기준 도달 시 취소선 표시
    ↓
모든 항목 완료 여부 검사
    ↓
Clear Panel 표시
```

### 관련 코드 요약

```csharp
public void IncrementCurrentToDoThingCount(ToDoList toDoList)
{
    currentToDoThingCountsList[(int)toDoList] += 1;

    CheckFinishToDoThing(toDoList);
    StartCoroutine(CheckToDoListState());
}
```

```csharp
public void UpdateToDoListTMPS(ToDoList toDoList)
{
    ToDoListTMPs[(int)toDoList].text =
        toDoListTitle[(int)toDoList] +
        $" ({currentToDoThingCountsList[(int)toDoList]}/{toDoThingFinishCriteriaList[(int)toDoList]})";

    if (toDoListDictionary[(int)toDoList])
    {
        ToDoListTMPs[(int)toDoList].text =
            "<s>" + ToDoListTMPs[(int)toDoList].text + "</s>";
    }
}
```

### 개선 포인트

할 일마다 완료 기준이 다르기 때문에,
현재 진행 개수와 완료 기준을 List로 분리하고 Enum 인덱스로 접근하도록 구현했습니다.

이를 통해 휴지 줍기, 음식물 줍기, 그릇 정리하기처럼 서로 다른 목표 개수를 가진 작업을
하나의 흐름으로 처리할 수 있었습니다.

<br>

---

<br>

## 5. Running Mini-game

### 핵심 코드

* `MiniGame3Manager.cs`
* `PlayerController.cs`
* `ScrollingBackground.cs`

### 구현 목적

사용자가 화면을 터치하면 캐릭터가 달리고, 반복 터치 시 일시적으로 속도가 증가하는 미니게임입니다.
달리기 연출을 위해 **캐릭터 애니메이션, 배경 스크롤, 목표 Slider, 경쟁자 이동**을 함께 제어했습니다.

### 주요 구현

* 시작 전 카운트다운 Coroutine 구현
* 목표 지점 도달을 Slider 진행도로 표현
* 터치 입력에 따른 달리기 시작 및 Burst 처리
* Animator 속도 조절
* RawImage UV Rect 기반 배경 스크롤 구현
* Lerp 기반 캐릭터 위치 이동
* 경쟁자 캐릭터 지연 이동 및 뒤처짐 연출
* 목표 도달 시 Clear Panel 표시

### Flow

```text
Guide Panel
    ↓
StartGame()
    ↓
3, 2, 1, 시작! 카운트다운
    ↓
목표 Slider 증가
    ↓
사용자 터치
    ↓
캐릭터 달리기 시작
    ↓
반복 터치 시 Burst
    ↓
배경 스크롤 속도 증가
    ↓
목표 도달
    ↓
Clear Panel 표시
```

### 관련 코드 요약

```csharp
public void HandleTouch()
{
    if (!hasStartedRunning)
    {
        hasStartedRunning = true;
        PlayerAnimator.Play("player_runningStart");

        Invoke(nameof(StartRunningLoop),
            PlayerAnimator.GetCurrentAnimatorStateInfo(0).length);

        miniGame3Manager.scrollingBackground.SetIsRunning(true);
    }
    else
    {
        if (isInBurst) return;
        StartCoroutine(BurstSpeed());
    }
}
```

```csharp
public void SetBurstSpeed(bool isBurst)
{
    if (isBurst)
        speed = 8f;
    else
        speed = 5f;
}
```

### 개선 포인트

터치 입력 하나가 단순한 수치 증가로 끝나지 않고,
애니메이션 속도, 배경 스크롤 속도, Slider 진행 속도, 경쟁자 위치 변화로 함께 연결되도록 구현했습니다.

이를 통해 짧은 미니게임 안에서도 사용자가 조작의 변화를 즉각적으로 느낄 수 있도록 구성했습니다.

<br>

---

<br>

## 6. Secret Dancing Mini-game

### 핵심 코드

* `MiniGame4Manager.cs`

### 구현 목적

사용자가 터치를 유지하면 춤 진행도가 증가하고, 터치를 멈추면 진행도가 감소하는 미니게임입니다.
중간에 어른 캐릭터가 등장하면 터치를 멈춰야 하며, 들키면 재시작되는 구조입니다.

### 주요 구현

* 터치 유지 여부에 따른 Slider 증가 및 감소
* 어른 등장 여부에 따른 위험 상태 관리
* Random 기반 방해 이벤트 발생
* 터치 중 어른 등장 시 Restart 처리
* Clear 조건 달성 시 게임 정지 및 Clear Panel 표시
* 캐릭터 오브젝트 활성화/비활성화
* 진행 상태에 따른 UI 색상 변경

### Flow

```text
StartGame()
    ↓
터치 입력 대기
    ↓
터치 유지 시 Progress 증가
    ↓
터치 해제 시 Progress 감소
    ↓
랜덤하게 어른 등장
    ↓
어른 등장 중 터치 유지 시 Restart
    ↓
Progress 100% 도달 시 Clear
```

### 관련 코드 요약

```csharp
private void UpdateProgressSlider()
{
    if (isTouching)
        progressSlider.value += Time.deltaTime / fillDuration;
    else
        progressSlider.value -= Time.deltaTime * decayRate;

    progressSlider.value = Mathf.Clamp01(progressSlider.value);
}
```

```csharp
private void TriggerRestart()
{
    if (isRestart) return;

    isRestart = true;
    isStart = false;
    SetRestartActive(true);
}
```

### 개선 포인트

단순 터치 반복이 아니라,
**터치 유지, 위험 상황 감지, 재시작 조건, 진행도 감소**를 함께 구성하여
사용자가 상황을 관찰하며 입력을 조절해야 하는 미니게임 흐름을 구현했습니다.

<br>

---

<br>

## 7. Touch Indicator & Alpha Hit Test

### 핵심 코드

* `TouchIndicatorController.cs`
* `AlphaImage.cs`

### 구현 목적

Android 환경에서 사용자의 터치 위치를 시각적으로 보여주고,
투명 PNG 이미지의 불투명 영역만 터치되도록 처리했습니다.

### 주요 구현

* 모바일 Touch Input과 Unity Editor Mouse Input 분기
* 터치 위치에 UI Indicator 표시
* 터치 종료 시 Indicator 비활성화
* `alphaHitTestMinimumThreshold`를 활용한 이미지 터치 영역 보정

### 관련 코드 요약

```csharp
#if UNITY_EDITOR
if (Input.GetMouseButton(0))
{
    Vector2 pos = Input.mousePosition;
    ShowIndicator(pos);
}
else
{
    HideIndicator();
}
#else
if (Input.touchCount == 1)
{
    Touch touch = Input.GetTouch(0);
    Vector2 touchPos = touch.position;

    if (touch.phase == TouchPhase.Began ||
        touch.phase == TouchPhase.Moved ||
        touch.phase == TouchPhase.Stationary)
    {
        ShowIndicator(touchPos);
    }
    else if (touch.phase == TouchPhase.Ended ||
             touch.phase == TouchPhase.Canceled)
    {
        HideIndicator();
    }
}
#endif
```

```csharp
public class AlphaImage : MonoBehaviour
{
    public float AlphaThreshold = 0.1f;

    void Start()
    {
        this.GetComponent<Image>().alphaHitTestMinimumThreshold = AlphaThreshold;
    }
}
```

### 개선 포인트

모바일 테스트뿐 아니라 Unity Editor에서도 마우스로 동일한 입력 흐름을 검증할 수 있도록 분기 처리했습니다.

또한 투명한 영역까지 버튼으로 인식되는 문제를 줄이기 위해 Alpha Hit Test를 적용하여
실제 이미지 모양에 가까운 터치 판정이 가능하도록 했습니다.

<br>

---

<br>

## 🔄 Overall Mini-game Flow

```text
Main Story
    ↓
Episode Mini-game Scene
    ↓
Guide Panel
    ↓
StartGame()
    ↓
User Touch / Object Interaction
    ↓
State Update
    ↓
UI Feedback
    ↓
Clear Condition Check
    ↓
Clear Panel
    ↓
BackToMainStory()
    ↓
Main Story Next Step
```

<br>

## 🧠 Technical Highlights

### 1. Mini-game별 독립 구조

각 미니게임은 독립된 Manager 클래스를 중심으로 구성했습니다.

* `MiniGame1Manager`
* `MiniGame2_1Manager`
* `MiniGame2_2Manager`
* `MiniGame3Manager`
* `MiniGame4Manager`

각 Manager는 해당 미니게임의 입력 처리, 상태 갱신, 완료 조건, UI 피드백을 담당하도록 구성했습니다.

<br>

### 2. Enum과 Dictionary 기반 상태 관리

음식 선택, 할 일 완료 여부 등 여러 상태를 관리해야 하는 기능에서는
Enum을 Key로 사용하고 Dictionary 또는 List를 함께 활용했습니다.

이를 통해 개별 변수를 여러 개 두는 방식보다 상태 접근과 완료 조건 검사를 단순화했습니다.

<br>

### 3. Coroutine 기반 시간 흐름 처리

제한 시간, 카운트다운, 클리어 후 복귀, 이동 연출 등 시간에 따라 진행되는 기능은 Coroutine으로 처리했습니다.

사용된 예시는 다음과 같습니다.

* 숨바꼭질 제한 시간
* 달리기 시작 전 카운트다운
* 캐릭터 Burst 이동
* 경쟁자 이동 연출
* 클리어 후 메인 스토리 복귀
* 랜덤 방해 이벤트

<br>

### 4. 아동 사용자를 고려한 피드백 구조

아동 대상 콘텐츠라는 프로젝트 목적에 맞춰,
사용자의 입력 결과가 즉시 시각적 피드백으로 이어지도록 구현했습니다.

예시는 다음과 같습니다.

* 정답 캐릭터 클릭 시 원형 표시
* 음식 선택 수에 따른 캐릭터 표정 변화
* 할 일 완료 시 진행도 텍스트와 취소선 표시
* 터치 위치 Indicator 표시
* 달리기 중 배경 속도 및 캐릭터 위치 변화
* 클리어 시 Clear Panel 표시

<br>

## ⚠️ Troubleshooting & Improvements

## Case 01. 투명 이미지 터치 판정 문제

### Problem

투명 PNG 이미지를 버튼으로 사용할 때, 실제 캐릭터가 없는 투명 영역까지 터치되는 문제가 발생할 수 있었습니다.

### Cause

Unity UI Image의 기본 Raycast Target은 이미지의 사각형 RectTransform 영역 전체를 터치 영역으로 인식합니다.

### Fix

`alphaHitTestMinimumThreshold`를 적용하여 이미지의 불투명한 영역만 터치되도록 조정했습니다.

### Result

숨은 캐릭터나 불규칙한 형태의 오브젝트를 터치할 때,
실제 이미지 형태에 가까운 판정이 가능하도록 개선했습니다.

<br>

---

<br>

## Case 02. 미니게임 상태 초기화 문제

### Problem

미니게임 재시작 시 이전 플레이에서 변경된 버튼 상태, 점수, Slider 값이 남아 있을 수 있었습니다.

### Cause

미니게임 시작 시 상태 초기화가 충분히 분리되어 있지 않으면,
이전 상태가 다음 플레이 흐름에 영향을 줄 수 있습니다.

### Fix

미니게임별로 `Init` 계열 메서드를 두고,
점수, Slider, 버튼 활성화 상태, 캐릭터 아이콘, 진행도 값을 시작 시점에 다시 초기화했습니다.

### Result

재시작 상황에서도 미니게임이 동일한 초기 조건에서 다시 실행되도록 개선했습니다.

<br>

---

<br>

## Case 03. 터치 입력과 피드백 연결 문제

### Problem

아동 사용자는 조작 결과가 즉시 보이지 않으면 현재 입력이 성공했는지 이해하기 어려울 수 있습니다.

### Cause

입력 처리만 있고 시각적 피드백이 부족하면, 사용자가 다음 행동을 판단하기 어렵습니다.

### Fix

터치 후 즉시 다음과 같은 피드백이 발생하도록 구성했습니다.

* 버튼 비활성화
* 정답 표시 이미지 활성화
* 캐릭터 표정 변경
* 할 일 진행도 텍스트 갱신
* Slider 증가
* 배경 스크롤 속도 변화
* 터치 위치 Indicator 표시

### Result

사용자의 입력이 게임 상태 변화로 바로 이어지도록 하여
아동 사용자가 조작 결과를 쉽게 이해할 수 있도록 개선했습니다.

<br>

## 📌 Key Code Files

| 파일명                           | 설명                                    |
| :---------------------------- | :------------------------------------ |
| `MiniGameManager.cs`          | 미니게임 공통 기반 클래스, 시작/종료/복귀 흐름 관리        |
| `MiniGame1Manager.cs`         | 숨바꼭질 미니게임의 제한 시간, 스테이지, 점수, 클리어 조건 관리 |
| `HiddenCharacterButton.cs`    | 숨은 캐릭터 버튼 클릭 처리 및 정답 표시               |
| `MiniGame2_1Manager.cs`       | 음식 선택 상태 관리 및 완료 조건 처리                |
| `FoodButton.cs`               | 음식 버튼 클릭 이벤트 처리                       |
| `CharacterReaction.cs`        | 선택된 음식 수에 따른 캐릭터 표정 변경                |
| `MiniGame2_2Manager.cs`       | 할 일 목록, 진행도, 완료 조건 관리                 |
| `DirtyDishButton.cs`          | 그릇 정리 오브젝트 클릭 처리                      |
| `TrashButton.cs`              | 쓰레기 오브젝트 클릭 처리                        |
| `MiniGame3Manager.cs`         | 달리기 미니게임의 카운트다운, 목표 Slider, 클리어 조건 관리 |
| `PlayerController.cs`         | 터치 기반 달리기, Burst, 캐릭터 이동 연출           |
| `ScrollingBackground.cs`      | RawImage UV Rect 기반 배경 스크롤            |
| `MiniGame4Manager.cs`         | 터치 유지형 몰래 춤추기 미니게임 진행 관리              |
| `TouchIndicatorController.cs` | 모바일 터치 위치 시각화 및 Editor 테스트 입력 분기      |
| `AlphaImage.cs`               | 투명 PNG 이미지의 Alpha 기반 터치 판정 보정         |

<br>

## 📊 User Test & Validation

본 프로젝트는 사용자 테스트용 Android 빌드를 제작하여
일반 초등학생 30명을 대상으로 사용성 및 정서 반응 검증을 진행했습니다.

테스트 과정에서는 다음 항목을 확인했습니다.

* 게임 문장 이해도
* 미니게임 난이도
* 사회적 학습 가능성
* 정서적 만족도
* 지속 플레이 의사
* 전반적 흥미도

미니게임 구현 과정에서 아동 사용자가 쉽게 이해할 수 있도록
짧은 플레이 흐름, 단순한 입력 구조, 즉각적인 피드백을 우선적으로 고려했습니다.

<br>

## 🧾 Publication

본 프로젝트는 아래 논문으로 게재되었습니다.

**경계선 지능 아동의 정서 및 사회 관계 형성 지원을 위한 얼굴 인식 기반 인터랙티브 게임**
*Facial Recognition-Based Interactive Game to Support Emotional and Social Relationship Formation in Borderline Intellectual Functioning Children*

* Journal: The Journal of the Convergence on Culture Technology (JCCT)
* Vol. 11, No. 6, pp. 321-327
* Published: 2025.11.30
* DOI: http://dx.doi.org/10.17703/JCCT.2025.11.6.321

<br>

## 🔗 Links

* **Project Repository**: https://github.com/limce106/HeyCheese
* **Demo Video**: https://youtu.be/4RVUEoXP8Yw?si=HcLrg0qtpwAvmj4N

<br>

## ✅ Summary

『헤이 치즈!』에서는 아동 대상 교육용 인터랙티브 콘텐츠에 필요한
**짧고 직관적인 미니게임 흐름, 터치 기반 상호작용, 상태 기반 피드백 구조**를 구현했습니다.

특히 여러 에피소드에 포함된 미니게임을 각각의 규칙에 맞게 구현하면서,
사용자의 입력이 즉각적인 UI 변화와 캐릭터 반응으로 이어지도록 구성했습니다.

이 프로젝트를 통해 단순한 기능 구현뿐 아니라,
사용자의 연령과 인지 수준을 고려한 인터랙션 설계와 테스트 빌드 검증의 중요성을 경험했습니다.

<br>

## 🏷️ Keywords

`Unity` `C#` `Android` `Mini-game Flow` `Touch Interaction`
`Coroutine` `Dictionary` `Unity UI` `TextMeshPro` `Slider`
`Sprite Feedback` `Lerp Movement` `Educational UX` `User Test Build`
