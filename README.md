# Unity Game Systems Portfolio

이 리포지토리는 **넥슨 인턴십 지원용 게임 프로그래머 포트폴리오**로,  
제가 참여한 팀 프로젝트 *『필연과 우연』* 과 *『네 발자국』* 에서 직접 구현한 **주요 시스템 코드**들을 정리한 것입니다.

## 📌 프로젝트 개요
- **『필연과 우연』** : 멀티엔딩 방탈출 어드벤처 게임  
  → 퍼즐, 상호작용 오브젝트, 행동력/날짜 시스템, 룸 매니저 등 핵심 게임플레이 시스템을 구현  

- **『네 발자국』** : 반려동물 유기를 주제로 한 2D 내러티브 어드벤처  
  → 대화(Dialogue), 이벤트(Event), 결과(Result) 처리 시스템을 데이터 주도 설계 기반으로 구현  

## 🛠️ 기여한 주요 시스템
- **Puzzle Systems** : 드래그 앤 드롭 퍼즐, 정답 검증 및 애니메이션 처리
- **Interaction Systems** : 공통 인터랙션(EventObject) + 개별 동작(Chair, Drawers)
- **Action Point System** : 추상 클래스 기반 행동력/날짜 전환 시스템
- **Room Manager** : 시점 전환, UI 제어, 튜토리얼 연동
- **Dialogue System** : 대사 진행, 텍스트 연출, 선택지 분기
- **Result & Event System** : CSV 파싱 기반 조건/결과 파이프라인 (Data-driven design)

## 📂 Quick Links
- [Puzzle Systems](./FateAndAccidy/PuzzleSystem/README.md)  
- [Interaction Systems](./FateAndAccidy/InteractionSystem/README.md)  
- [Action Point System](./FateAndAccidy/ActionPointSystem/README.md)  
- [Room Manager](./FateAndAccidy/RoomManager/README.md)  
- [Dialogue System](./FourFootsteps/DialogueSystem/README.md)  
- [Result System](./FourFootsteps/ResultSystem/README.md)  
- [Event System](./FourFootsteps/EventSystem/README.md)  
