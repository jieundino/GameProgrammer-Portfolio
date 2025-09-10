# Interactable Objects

플레이어가 클릭하거나 조사할 수 있는 상호작용 오브젝트 관리 코드입니다.
공통 인터랙션 베이스 클래스를 설계하고 이를 상속하여 다양한 오브젝트 동작을 구현했습니다.

## 주요 기능
- **EventObject**
  - 공통 인터랙션 처리 (조사/이벤트 분기)
  - EventManager와 GameManager 연동
- **Chair**
  - 의자 이동 애니메이션
  - 상태 동기화 (ChairMoved 변수 반영)
- **Drawers**
  - 서랍 열림/닫힘 상태 관리
  - Coroutine 기반 슬라이드 이동

## 구조 다이어그램
[EventObject] (Base Class)  
└─ OnMouseDown() → EventManager 호출

[Chair] (EventObject 상속)  
└─ ExecuteAction() → MoveChair Coroutine  
└─ 상태 동기화 (ChairMoved)

[Drawers] (EventObject 상속)  
└─ ExecuteAction() → ToggleDoors()  
└─ Coroutine으로 슬라이드 애니메이션

## 요약
공통 상호작용 기반을 설계하고 이를 상속받아 **의자/서랍 이동 애니메이션과 상태 전환**을 구현했습니다.