# Unity Game Systems Portfolio

Unity C# 기반 게임 클라이언트 프로그래머 포트폴리오입니다.  
제가 참여한 팀 프로젝트 『필연과 우연』과 『네 발자국』에서 직접 설계하고 구현한 주요 시스템 코드를 정리했습니다.

---

## 📌 프로젝트 개요

### 『필연과 우연』
멀티엔딩 방탈출 어드벤처 게임  
퍼즐, 상호작용 오브젝트, 행동력/날짜 시스템, 사운드 시스템 등 핵심 게임플레이 시스템을 설계하고 구현했습니다.  
**Stove, App Store 출시** | 2025 BIC 전시 참여

### 『네 발자국』
반려동물 유기를 주제로 한 2D 내러티브 어드벤처  
대화(Dialogue), 이벤트(Event), 결과(Result) 처리 시스템을 데이터 주도 설계 기반으로 구현했습니다.  
학술 연구용 플레이 로그 수집 시스템을 직접 설계·구현했습니다.  
**Stove 출시** | 학술저널 제1저자 게재 (2026.05 예정)

---

## 🛠️ 기여한 주요 시스템

| 시스템 | 프로젝트 | 핵심 기술 |
|--------|---------|----------|
| Puzzle Systems | 필연과 우연 | Unity UI 이벤트, 드래그 앤 드롭, 좌표 변환, 제약 로직 |
| Interaction Systems | 필연과 우연 | 공통 인터페이스 설계, Lerp 이동 애니메이션, 세이브 연동 |
| Action Point System | 필연과 우연 | 추상 클래스 + 상속, 코루틴 기반 날짜 전환 애니메이션 |
| Room Manager | 필연과 우연 | 다중 상태 통합 관리, UIManager 동기화, 입력 방지 |
| Sound System | 필연과 우연 | 우선순위 채널, 보이스 스틸링, 라운드 로빈, 디바운스 |
| Dialogue System | 네 발자국 | Queue 기반 비동기 제어, 타자 효과, 말풍선, 선택지 분기 |
| Event & Result System | 네 발자국 | CSV 파싱, 데이터 주도 설계, AND/OR 조건, 코루틴 파이프라인 |
| Save System | 네 발자국 | Atomic Write, Newtonsoft.Json, 타입 정보 보존, Fallback 복구 |
| Log Tracking System | 네 발자국 | UnityWebRequest, Queue 재전송, GAS 연동, 중복 방지 |

---

## 📂 Quick Links

### 『필연과 우연』 (FateAndAccidy)
- [Puzzle Systems](./FateAndAccidy/PuzzleSystems/README.md)
- [Interaction Systems](./FateAndAccidy/InteractableObjects/README.md)
- [Action Point System](./FateAndAccidy/ActionPointSystem/README.md)
- [Room Manager](./FateAndAccidy/RoomManager/README.md)
- [Sound System](./FateAndAccidy/SoundSystem/README.md)

### 『네 발자국』 (FourFootsteps)
- [Dialogue System](./FourFootsteps/DialogueSystem/README.md)
- [Event & Result System](./FourFootsteps/Event&ResultSystem/README.md)
- [Save System](./FourFootsteps/SaveSystem/README.md)
- [Log Tracking System](./FourFootsteps/LogSystem/README.md)

---

## 🎬 유튜브 시연 영상

- [『필연과 우연』](https://youtu.be/kjaH9fDRmMo)
- [『네 발자국』](https://youtu.be/013OU2ZJlbk)
