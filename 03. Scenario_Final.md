__로봇 협업 시나리오 문서__

Leader Robot \+ Follow Robot 최종 시나리오

__작성 목적__

본 문서는 Leader 로봇과 Follow 로봇, 두 로봇의 협업 시나리오를 다룬 문서임을 공시한다\.

# 1\. 시스템 구성 개요

전체 시스템은 Leader 로봇과 Follow 로봇이 공유 TP\(Target Point\) 상태 정보를 통해 서로의 이동과 작업 타이밍을 맞춘 구조다\. Leader 로봇은 물체 집기, 하역, 최종 주차를 담당하고, Follow 로봇은 Leader 추종 및 Pallet Lifting 상차·하차 작업과 TP1/TP2/TP3 동기화를 담당한다\.

__구성 요소__

__역할__

Leader 로봇

Nav2 기반 주행, Marker 접근, 집기, 하역, 최종 주차

Follow 로봇

Leader 로봇 Marker 추종, TP1/TP2/TP3 도착 판정, 반대편 Pallet Lifting 작업 수행

공유 TP 노드

tp1, tp2, tp3 상태 보관, Ready \-> Go \-> Arrive 흐름 동기화  
\- tp1: 최초 작업 분기/동기화 지점  
\- tp2: 1차 작업 완료 후 재합류/동기화 지점  
\- tp3: 최종 작업 전 재합류 및 하역 허가 동기화 지점

Marker 인식

작업 위치 접근 및 최종 정렬

# 2\. 핵심 동기화 개념

TP1, TP2, TP3는 로봇의 작업위치 및 정렬위치를 나타내며, Ready \-> Go \-> Arrive 순으로 상태 변경에 따라 이동, 분기, 정렬처리를 진행하며, 로봇 충돌을 방지하고 Leader, Follower 역할을 나눠 작업에 문제없이 진행될 수 있게 도와주는 트리거 역할이 핵심 요소이다\.

__상태__

__의미__

__운영상 해석__

ready

초기 대기 상태

각 TP 지점 이동 전, 준비 상태

go

상대 로봇에게 다음 행동을 허가

각 TP 지점 이동 트리거

arrive

해당 구간 도착/정렬/작업 준비 완료

Follow 로봇이 각 TP 구간의 도착·정렬 조건을 만족한 상태

# 3\. 전체 임무 흐름

전체 임무는 TP1 추종, 작업 분기, TP2 재 합류, TP3 하역 준비, 최종 하역 후 주차 순서로 진행된다\.

- 초기 상태에서 모든 TP 값은 ready로 진행한다\.
- Follow 로봇은 Leader 로봇의 Marker를 안정적으로 인식하고 사전 설정된 추종 거리까지 접근하여 추종 준비가 완료되면 TP1을 go로 변경한다\.
- Leader 로봇은 TP1 go를 확인한 뒤 집기 대기 지점으로 이동한다\.
- Follow 로봇은 Leader 로봇의 Marker를 따라가며 일정 거리 접근 후 정지 조건을 만족하면 TP1을 arrive로 변경하여 작업 위치 이동을 준비한다\.
- Leader 로봇은 TP1 arrive를 확인한 뒤 우측 Marker 작업 구간에서 물체를 집고 복귀하며, Follow 로봇은 좌측 Marker 작업 구간에서 상차 후 TP2 상태를 계속 확인한다\.
- Leader 로봇은 집기 작업 후 복귀 정렬 지점에 도착하면 TP2를 go로 변경하고, Follow 로봇의 TP2 arrive를 기다리며 대기한다\.
- Follow 로봇이 TP2 이동을 완료하면 TP2를 arrive로 변경한다\. Leader 로봇은 TP2 arrive를 확인한 뒤 하역 대기 지점으로 이동하며, Follow 로봇은 반대편 작업 구간에서 Pallet 하차 작업을 수행한 뒤 복귀한다\.
- Leader 로봇은 하역 대기 지점 도착 후 TP3를 go로 변경하고 Follow 로봇의 TP3 arrive를 기다린다\. Follow 로봇은 TP3 go를 확인한 뒤 Leader 로봇을 추종하여 최종 동기화 위치로 이동한다\.
- TP3 arrive 이후 Leader 로봇은 좌측 Marker 작업 구간에서 하역하고 후진한 뒤, 주차 Marker를 Tracking하여 최종 주차 위치로 이동 후 주차한다\. Follow 로봇은 TP3 arrive 이후 지정된 최종 위치에서 임무를 완료한다\.

# 4\. 단계별 시나리오 상세

__단계__

__구간__

__TP 상태__

__로봇 동작__

__점검 포인트__

초기 대기

tp1/tp2/tp3 = ready

Leader, Follow 로봇 모두 대기

공유 TP 노드가 정상 실행

TP1 이동 시작

Follow 로봇 tp1=go

Leader 로봇이 작업 분기 지점으로 이동

Leader 로봇 Marker가 안정적으로 보이도록 주행 경로와 방향을 유지

Marker 추종

Follow 로봇이 Leader 로봇 Marker를 추종

안전 거리 안으로 접근하면 감속 또는 정지하고, 거리 확보 시 추종 재개

TP1 추종 완료

Follow 로봇 tp1=arrive

Leader 로봇은 우측 작업 Marker 접근 시작  
Follow 로봇은 좌측 작업 구간 진입 준비

Leader 로봇 정지 여부와 두 로봇 간 안전 거리 확인

작업 위치 이동

각 로봇이 서로 반대편 작업 Marker를 추종하여 이동

작업 위치 Marker 인식 거리와 각도 안정성이 중요

1차 작업

Leader 로봇  
\- 집게 벌림 → Marker 접근 → 집기 → 후진  
  
Follow 로봇  
\- Pallet Lifting up → 작업 완료 후 복귀 준비

집게 및 Lifting 동작을 사전에 수동 점검

TP2 합류 시작

Leader 로봇 tp2=go

Leader 로봇이 집기 후 복귀 정렬 지점에 먼저 도착하고 tp2=go 전송 후 대기

Leader 로봇은 TP2 arrive 전까지 다음 구간으로 이동하지 않음

TP2 이동 완료

Follow 로봇 tp2=arrive

Follow 로봇이 TP2 위치로 복귀하여 arrive 응답  
Leader 로봇은 TP2 arrive 확인 후 하역 대기 지점으로 이동

두 로봇의 이동 경로가 겹치지 않는지 확인

Follow 2차 작업 / Leader 하역 위치 이동

Follow 로봇은 반대편 작업 Marker 구간에서 Pallet Lifting down 수행 후 복귀  
Leader 로봇은 하역 대기 지점으로 이동

각 로봇 작업 공간과 이동 경로 분리 확인

TP3 이동 시작

Leader 로봇 tp3=go

Leader 로봇이 하역 대기 지점 도착 후 tp3=go 전송  
Follow 로봇은 Leader 로봇 추종 시작

Leader 로봇은 TP3 arrive 전까지 하역 Marker 접근 금지

TP3 이동 완료

Follow 로봇 tp3=arrive

Follow 로봇이 최종 동기화 위치 도착  
Leader 로봇은 TP3 arrive 확인 후 하역 작업 시작

Leader 정지 확인 및 두 로봇 간 안전 거리 확인

하역 작업

Leader 로봇  
\- 좌측 Marker 접근 → 물체 하역 → 후진  
  
Follow 로봇  
\- 지정된 최종 위치에서 임무 완료

하역 위치 Marker 인식 거리와 각도 안정성이 중요

Leader 최종 주차

Leader 로봇이 큰 Marker로 방향을 정렬한 뒤 작은 Marker를 기준으로 최종 주차

2단계 주차 Marker 인식 및 기준선 정렬 상태 확인

# 5\. Leader 로봇 관점의 동작 해석

Leader 로봇은 단독 주행 로봇이 아니라, Follow 로봇의 도착 신호를 계속 기다리며 다음 행동을 결정한다\. 특히 TP1에서는 Follow 로봇이 먼저 go를 만들고, TP2와 TP3에서는 Leader 로봇이 go를 만들어 Follow 로봇을 호출하는 구조다\.

- TP1에서는 Follow 로봇이 Leader 로봇 추종을 시작시키는 주체다\.
- TP1 arrive 이후 Leader 로봇은 우측 작업 공간에서 집기 작업을 수행한다\.
- 집기 후 바로 장거리 주행하지 않고 먼저 후진하여 장애물 영역에서 빠져나온다\.
- 복귀 지점 도착 후 TP2 go를 전송하고, Follow 로봇이 TP2 arrive를 줄 때까지 움직이지 않는다\.
- 하역 대기 지점 도착 후 TP3 go를 전송하고, Follow 로봇의 TP3 arrive를 기다린다\.
- 하역 후 별도 복귀 없이 주차 Marker 탐색으로 이어진다\.

# 6\. Follow 로봇 관점의 동작 해석

Follow 로봇 흐름은 Leader 로봇 추종과 TP 상태 변경을 중심으로 Leader 로봇 임무와 결합된다\. 핵심은 Follow 로봇이 Leader 로봇을 무조건 따라가는 것이 아니라, 거리 조건과 정지 조건을 만족할 때만 arrive를 발생시켜 Leader 로봇의 다음 작업을 허가한다는 점이다\.

- TP1: Follow 로봇은 Leader 로봇 Marker를 안정적으로 인식하고 추종 준비를 완료한 뒤 go를 만들고, Leader 로봇을 따라간다\.
- TP1 추종 중 Leader 로봇과 가까워지면 감속하거나 정지하여 추돌을 방지하고, 안전 거리가 다시 확보되면 추종을 재개한다\.
- TP1 완료 시점은 단순 위치 도착이 아니라 Leader 로봇 정지 확인과 안전 거리 확보가 결합된 상태다\.
- TP2: Leader 로봇이 복귀 후 go를 만들면 Follow 로봇은 TP2 위치로 이동하여 arrive로 응답하고, 이후 반대편 작업 구간에서 Pallet 하차 작업을 수행한 뒤 복귀한다\.
- TP3: Leader 로봇이 하역 대기 지점 도착 후 go를 만들면 Follow 로봇은 Leader 로봇을 추종하여 최종 동기화 위치로 이동하고 arrive로 응답한다\.
- 따라서 Follow 로봇은 Leader 로봇의 보조 로봇이면서 동시에 Leader 로봇의 작업 허가 조건을 제공하는 안전 인터락 역할을 한다\.

# 7\. 방향 및 작업 공간 해석

현재 시나리오는 Leader 로봇이 처음에는 전방을 보고 이동하고, 집기 후 복귀하면서 반대 방향을 보도록 설계되어 있다\. 이 때문에 같은 벽이나 작업 구간을 대상으로 하더라도 집기와 하역에서 탐색 방향이 반대로 해석된다\.

__구간__

__Leader 로봇 방향__

__탐색 방향__

__목적__

집기 구간

전방 방향

우측 Marker 탐색

물체 접근 후 집기

복귀 구간

반대 방향

주 경로로 복귀

Follow 로봇 TP2 합류 완료 대기

하역 구간

반대 방향

좌측 Marker 탐색

물체 내려놓기

주차 구간

주차 방향

주차 Marker 탐색 및 정렬

최종 정렬 및 종료

# 8\. 충돌 방지 관점의 핵심

두 로봇이 같은 공간에서 움직이기 때문에 가장 중요한 것은 속도보다 순서 제어다\. 현재 구조는 TP 상태를 이용해 서로의 경로를 막지 않도록 설계되어 있다\.

- Leader 로봇은 TP1에서 Follow 로봇이 도착했다고 판단하기 전까지 집기 Marker 작업을 시작하지 않는다\.
- Leader 로봇은 TP2 arrive를 받기 전까지 하역 대기 지점으로 이동하지 않는다\.
- Leader 로봇은 TP3 arrive를 받기 전까지 하역 Marker 접근을 시작하지 않는다\.
- Follow 로봇은 Leader 로봇 추종 중 안전 거리 안으로 들어오면 감속 또는 정지하고, 안전 거리가 확보되면 추종을 재개해야 한다\.
- TP 상태가 중복 노드에서 관리되면 go와 arrive가 불안정하게 보일 수 있으므로 TP 소유 노드는 하나만 유지해야 한다\.

# 9\. 실제 구동 전 확인 사항

__항목__

__확인 내용__

__위험 요소__

TP 노드

tp1/tp2/tp3 소유 노드가 하나만 실행되는지 확인

go와 arrive 값이 흔들리면 가장 먼저 확인

Namespace

각 로봇의 odom, camera, marker, cmd\_vel이 섞이지 않는지 확인

두 로봇이 같은 ROS\_DOMAIN\_ID를 쓰면 특히 중요

Marker ID

Leader 로봇 집기/하역 Marker, 주차 Marker가 현장 배치와 일치하는지 확인

잘못된 ID 인식 시 엉뚱한 방향으로 접근

집게 방향

open/close 문자열과 실제 하드웨어 동작이 반대로 맵핑되어 있는지 확인

수동 테스트와 자동 동작 해석이 헷갈릴 수 있음

속도

Follow 로봇 추종 속도와 Leader 로봇 Nav2 속도 차이를 확인

추돌 방지 거리 조건 필요

주차

큰 Marker와 작은 Marker가 같은 주차 기준선에 배치되어 있는지 확인

2단계 정렬 성공률에 직접 영향

# 10\. 최종 결론

현재 결합 시나리오는 TP 상태를 중심으로 Leader 로봇과 Follow 로봇이 서로의 완료 상태를 확인하며 진행하는 구조다\. 큰 흐름은 일관성이 있으며, TP1에서는 Follow 로봇이 Leader 로봇을 출발시키고, TP2와 TP3에서는 Leader 로봇이 Follow 로봇을 호출하는 상호 handshake 구조로 정리된다\.

최종적으로 이 시나리오는 “Leader 로봇 단독 이동 \+ Follow 로봇 단순 추종”이 아니라, 두 로봇이 작업 공간을 나누고 TP 상태를 통해 서로의 행동 가능 시점을 제어하는 협업 임무 구조로 보는 것이 정확하다\. 실제 구동 전에는 topic namespace 분리, TP 노드 단일화, 거리 기반 추종 안전 로직을 우선 검증해야 한다\.

