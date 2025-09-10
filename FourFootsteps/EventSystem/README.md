# Event System

이벤트(Event) 실행 시스템 구현 코드입니다.  
조건(Condition)과 결과(Result)를 연결하여, 특정 상황에서 어떤 결과를 실행할지 결정합니다.

## 주요 기능
- **EventManager**
  - `events.csv` 파싱 → Event 데이터 로드
  - CallEvent()로 이벤트 실행
  - 조건 로직 지원:
    - 단일 조건
    - AND / OR 복합 조건
  - 실행 모드:
    - Instant (즉시 실행)
    - Sequential (순차 실행, Coroutine)

## 구조 다이어그램
[EventManager]  
└─ events (Dictionary) : CSV 파싱 데이터  
└─ CallEvent(eventID)  
   ├─ 조건 검사 (CheckConditions_AND / OR)  
   └─ ExecuteResults()  
       ├─ Instant → 병렬 실행  
       └─ Sequential → 순차 실행 (Coroutine)  

## 요약
**조건 + 결과 파이프라인**을 데이터로 정의해  
게임 이벤트를 유연하게 확장 가능하도록 설계했습니다.  
**조건 판정 → 결과 실행 → 연출 동기화** 흐름을 안정적으로 관리할 수 있습니다.
