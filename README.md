# Unity Game Systems Portfolio

Unity C# 기반 게임 클라이언트 프로그래머 포트폴리오입니다.
제가 참여한 팀 프로젝트 『필연과 우연』, 『네 발자국』, 『헤이 치즈!』에서 직접 구현한 주요 시스템 코드를 정리했습니다.

---

## 📌 프로젝트 개요

### 『필연과 우연』
<img src="https://github.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/FateAndAccidy_logo.png?raw=true" width="100"/>

#### 📱 Screenshots

| ![FateAndAccidy_Screenshot1](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fate_1.png) | ![FateAndAccidy_Screenshot2](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fate_2.png) | ![FateAndAccidy_Screenshot3](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fate_3.png) |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |

**멀티엔딩 방탈출 어드벤처 게임** <br>
퍼즐, 상호작용 오브젝트, 행동력/날짜 시스템, 사운드 시스템 등 핵심 게임플레이 시스템을 설계하고 구현했습니다. <br>
**Stove, App Store 출시** | 2025 BIC 전시 참여

### 『네 발자국』
<img src="https://github.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/FourFootsteps_logo.png?raw=true" width="100"/>

#### 📱 Screenshots

| ![FourFootsteps_Screenshot1](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fourfootsteps_1.jpg) | ![FourFootsteps_Screenshot2](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fourfootsteps_2.jpg) | ![FourFootsteps_Screenshot3](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/fourfootsteps_3.jpg) |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |

**반려동물 유기**를 주제로 한 **2D 내러티브 어드벤처** <br>
대화(Dialogue), 이벤트(Event), 결과(Result) 처리 시스템을 데이터 주도 설계 기반으로 구현했습니다.
학술 연구용 플레이 로그 수집 시스템을 직접 설계·구현했습니다.<br>
**Stove 출시** | 학술저널 제1저자 게재 (2026.05)

### 『헤이 치즈!』
<img src="https://github.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/HeyCheese_logo.png?raw=true" width="100"/>

#### 📱 Screenshots

| ![HeyCheese_Screenshot1](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/heycheese_1.jpg) | ![HeyCheese_Screenshot2](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/heycheese_2.jpg) | ![HeyCheese_Screenshot3](https://raw.githubusercontent.com/jieundino/GameProgrammer-Portfolio/blob/main/readme/heycheese_3.jpg) |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |

**경계선 지능 아동의 감정 표현**과 **사회적 관계 형성**을 돕는 **Android 기반 교육용 인터랙티브 콘텐츠**<br>
에피소드형 미니게임의 진행 흐름, 터치 기반 오브젝트 상호작용, 상태 기반 피드백 구조를 구현했습니다.<br>
**사용자 테스트용 빌드 제작** | JCCT 학술논문 제2저자 게재

---

## 🛠️ 기여한 주요 시스템

| 시스템                           | 프로젝트   | 핵심 기술                                                    |
| ----------------------------- | ------ | -------------------------------------------------------- |
| Puzzle Systems                | 필연과 우연 | Unity UI 이벤트, 드래그 앤 드롭, 좌표 변환, 제약 로직                     |
| Interaction Systems           | 필연과 우연 | 공통 인터페이스 설계, Lerp 이동 애니메이션, 세이브 연동                       |
| Action Point System           | 필연과 우연 | 추상 클래스 + 상속, 코루틴 기반 날짜 전환 애니메이션                          |
| Room Manager                  | 필연과 우연 | 다중 상태 통합 관리, UIManager 동기화, 입력 방지                        |
| Sound System                  | 필연과 우연 | 우선순위 채널, 보이스 스틸링, 라운드 로빈, 디바운스                           |
| Dialogue System               | 네 발자국  | Queue 기반 비동기 제어, 타자 효과, 말풍선, 선택지 분기                      |
| Event & Result System         | 네 발자국  | CSV 파싱, 데이터 주도 설계, AND/OR 조건, 코루틴 파이프라인                  |
| Save System                   | 네 발자국  | Atomic Write, Newtonsoft.Json, 타입 정보 보존, Fallback 복구     |
| Log Tracking System           | 네 발자국  | UnityWebRequest, Queue 재전송, GAS 연동, 중복 방지                |
| Mini-game Flow System         | 헤이 치즈! | 추상 MiniGameManager 기반 미니게임 시작, 클리어, 재시작, 메인 스토리 복귀 흐름    |
| Touch Interaction Mini-games  | 헤이 치즈! | 터치 기반 캐릭터 찾기, 음식 선택, 오브젝트 정리, 진행도 조작 구현                  |
| State-based Feedback System   | 헤이 치즈! | Dictionary, Slider, UI Text, Sprite 변경을 활용한 상태 기반 피드백 처리 |
| Coroutine-based Game Progress | 헤이 치즈! | 제한 시간, 카운트다운, 이동 연출, 랜덤 이벤트, 클리어 조건 검사 구현                |

---

## 📂 Quick Links

### 『필연과 우연』 (FateAndAccidy)

* [Puzzle Systems](./FateAndAccidy/PuzzleSystems/README.md)
* [Interaction Systems](./FateAndAccidy/InteractableObjects/README.md)
* [Action Point System](./FateAndAccidy/ActionPointSystem/README.md)
* [Room Manager](./FateAndAccidy/RoomManager/README.md)
* [Sound System](./FateAndAccidy/SoundSystem/README.md)

### 『네 발자국』 (FourFootsteps)

* [Dialogue System](./FourFootsteps/DialogueSystem/README.md)
* [Event & Result System](./FourFootsteps/Event&ResultSystem/README.md)
* [Save System](./FourFootsteps/SaveSystem/README.md)
* [Log Tracking System](./FourFootsteps/LogSystem/README.md)

### 『헤이 치즈!』 (HeyCheese)

* [Mini-game Interaction System](./HeyCheese/README.md)

---

## 🎬 유튜브 시연 영상

* [『필연과 우연』](https://youtu.be/kjaH9fDRmMo)
* [『네 발자국』](https://youtu.be/013OU2ZJlbk)
* [『헤이 치즈!』](https://youtu.be/4RVUEoXP8Yw?si=HcLrg0qtpwAvmj4N)
