# Dialogue System

대사 및 선택지 시스템 구현 코드입니다.  
플레이어와 NPC 간의 대화, 컷씬 이미지, 텍스트 연출, 선택지 분기를 모두 통합 관리합니다.

## 주요 기능
- **DialogueManager**
  - 대사 진행 관리 (시작, 종료, 큐잉)
  - 대화 모드 지원 (플레이어/ NPC 말풍선, 내적 독백, 일반 대화창)
  - 텍스트 연출: 타자 효과, 자동 진행, 페이드 아웃, 흔들림
  - 컷씬 이미지 및 캐릭터 초상화 전환
  - 선택지 UI 생성 및 선택 분기 처리

## 구조 다이어그램
[DialogueManager]

└─ dialogues (Dictionary) : 대사 데이터  
└─ choices (Dictionary) : 선택지 데이터  
└─ StartDialogue() : 대사 시작  
└─ DisplayDialogueLine() : 텍스트/이미지 세팅  
└─ TypeSentence() : 타자 효과 코루틴  
└─ DisplayChoices() → OnChoiceSelected()  

## 요약
**대사·컷씬·선택지·연출**을 하나의 모듈로 통합하여,  
게임 내 스토리 진행을 안정적으로 관리할 수 있는 대화 시스템을 구현했습니다.
