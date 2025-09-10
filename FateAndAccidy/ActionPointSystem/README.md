# Action Point System

행동력(하트)과 날짜 전환 시스템입니다.
공통 베이스 클래스와 방(Room)별 확장 클래스로 나누어 설계했습니다.

## 주요 기능
- **ActionPointManager (Base)**
  - 하트 배열 생성 및 관리
  - 날짜 전환 애니메이션 (기어 회전, 페이지 넘김, 페이드 효과)
  - 공통 추상 메서드 정의
- **Room1ActionPointManager**
  - 기본 방 로직 (행동력 소진 시 엔딩 처리, 다음날 전환)
- **Room2ActionPointManager**
  - 특수 아이템(회복제) 사용 시 하트 2개 보너스
  - 곰인형 이벤트로 행동력 규칙 확장

## 구조 다이어그램
[ActionPointManager] (Abstract)  
├─ CreateHearts()  
├─ DecrementActionPoint()  
├─ RefillHeartsOrEndDay()  
└─ Day Animation (기어/시계, 페이드)

[Room1ActionPointManager]  
└─ 기본 로직: 행동력 소진 → 엔딩 or 다음날

[Room2ActionPointManager]  
└─ 확장 로직: 회복제 아이템, 곰인형 이벤트

## 요약
추상 클래스 기반으로 **행동력/하트 소모와 날짜 전환 시스템**을 설계하고,
방마다 다른 규칙을 **상속 구조로 확장**했습니다.