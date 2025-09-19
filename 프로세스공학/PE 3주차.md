
비즈니스 프로세스는 여러 activity로 구성된다
week2-2
페트리넷 기반 프로세스 모델링 도구
Petri Nets
- 동시에 일어나는 이벤트에 효과적
- 자동 분석하는 툴이 많음
- 구성
	- Places: 원형
	- Transitions: 직선
	- Directed Arcs
	- Tokens: 작업의 주체
	- marking: 토큰의 개수 -> (3, 0)
	- Enable: input place에 요소가 있다면 해당 transiton이 true
	- Firing: enable된 place에서 토큰이 output place로 간 것
	- Firing Swquences: firing 된 순서
	- Actiive(Token) and Passive Components
	- Successiveness 연속성

![[Pasted image 20250915150422.png]]
(P1, P2) = (3, 0)
(P1, P2, P3, P4, P5) = (3, 0, 0, 0, 0) =transition firing=> (2. 1. 0. 1. 1)

Routing Patterns
- Sequential routing 
- Parallel routing 
- Selective routing 
- Iterative routing

High-Level Petri Nets
- Weighted Petri Net: enable이 되는 가중치를 둠
- Timed Petri Nets: 시간을 조절
- Hierarchical Petri Nets
- Colored Petri Nets

BPMN principle

Business Process Model and Notation
구성요소
- Activities
	- marker
	- task types
	- flow types
- Gateways
- Data
- Event
- Swimlanes: role마다 영역(Pool)을 나눔
- Choreographies: 어떤 관계인지 표시
- Converstions

conformance: 표준을 얼마나 잘 따랐는지 나타내는 지표
Flow objects: 구성하는 기본 요소, event, activities, gateway
Artifacts: flow, data
Connecting objects: sequence flow, message flow, association
![[Pasted image 20250915153537.png]]

이벤트 시작: 원, 이벤트 끝: 굵은 원, 중간 이벤트: 이중 원

task types
compensation task: transaction과 같이 취소가 불가능한 activity경우 보상의 느낌으로 새로운 일로 덮어 원래대로 돌리는 개념

event
blanco event: 빈 원, 이벤트의 원인을 모를 때 비워둔 이벤트