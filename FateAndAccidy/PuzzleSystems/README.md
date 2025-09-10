# Puzzle Systems

퍼즐 시스템 구현 코드입니다.
플레이어는 반짇고리 퍼즐에서 드래그 앤 드롭으로 비즈를 올바른 위치에 배치해야 클리어할 수 있습니다.

## 주요 기능
- **SewingBoxPuzzle**
  - 정답 테이블 기반 퍼즐 검증
  - 정답 시 게임 변수 업데이트 및 이벤트 호출
- **SewingBoxBead**
  - 드래그 앤 드롭 이벤트 처리 (시작, 이동, 종료)
  - 드롭존 계산 및 제약 조건 검사
  - 부드러운 이동 애니메이션

## 구조 다이어그램
[SewingBoxPuzzle]  
└─ 관리: BeadsAnswer(정답)  
└─ CompareBeads() → 정답 판정 → EventManager 호출

[SewingBoxBead]  
└─ Drag & Drop 이벤트 (OnBeginDrag, OnDrag, OnEndDrag)  
└─ DropZone 계산 및 제약 조건 검사  
└─ SmoothMoveToParent()로 애니메이션 이동

## 요약
UI 이벤트 기반 퍼즐을 제작하여 **정답 검증, 드롭 제약, 애니메이션 처리**를 종합적으로 구현했습니다.
