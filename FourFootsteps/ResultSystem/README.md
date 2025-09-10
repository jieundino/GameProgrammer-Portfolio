# Result System

결과(Result) 처리 시스템 구현 코드입니다.  
게임 기획 데이터(CSV)에 정의된 Result ID를 해석하여 다양한 연출, 변수 조작, 씬 전환을 실행합니다.

## 주요 기능
- **ResultManager**
  - `results.csv` 파싱 → Result 데이터 로드
  - ExecuteResultCoroutine()으로 ID별 결과 실행
  - 지원 기능:
    - 대사 시작 (StartDialogue)
    - 변수 증감/반전
    - 페이드 인/아웃, 컷씬 연출
    - 씬 전환, 튜토리얼 점프
    - 퍼즐 애니메이션 실행 (IResultExecutable 인터페이스)

## 구조 다이어그램
[ResultManager]  
└─ results (Dictionary) : CSV 파싱 데이터  
└─ RegisterExecutable() : 오브젝트 등록  
└─ ExecuteResultCoroutine()  
   ├─ Dialogue 시작  
   ├─ 변수 증감/반전  
   ├─ 페이드 인/아웃  
   ├─ 씬 이동/퍼즐 애니메이션  

## 요약
CSV 기반 **데이터 주도 설계(Data-driven design)**로,  
코드 수정 없이도 기획자가 Result ID를 추가/수정해  
**연출·변수 조작·씬 전환**을 자유롭게 확장할 수 있는 구조를 만들었습니다.
