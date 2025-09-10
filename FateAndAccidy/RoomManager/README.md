# Room & UI Manager

플레이어의 시점 전환, 확대 화면, UI 버튼 상태 등을 관리하는 핵심 매니저입니다.

## 주요 기능
- 시점 이동 (`MoveSides`)
- 확대/조사 상태 관리 (`isInvestigating`, `isZoomed`)
- UI 버튼 표시 규칙 (`SetButtons`)
- 행동력 시스템과 연동 (`ActionPointManager`)
- 무한 입력 방지 (`ProhibitInput`)

## 구조 다이어그램
[RoomManager]  
├─ MoveSides() → 시점 전환, 튜토리얼 연동  
├─ OnExitButtonClick() / ExitToRoot()  
├─ SetButtons() → UIManager와 상태 동기화  
└─ ProhibitInput() → 무한 입력 방지

## 요약
**시점 전환과 UI 제어를 통합 관리**하여
조사 상태·확대 상태·튜토리얼 진행 여부에 따라 유연하게 UI와 조작을 동기화했습니다.