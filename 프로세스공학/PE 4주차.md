
BPMN

Activities
- task
- subprocess

Event
- start: 얇은 선으로 된 원으로 표현
- end: 굵은 선으로 된 원으로 표현
- intermediate
	- interrupting: 이벤트가 발생한 즉시 프로세스가 멈추고 수행해야 하는 이벤트 ex)update, delete
	- non-interrupting: 프로세스가 멈출 필요 없이 처리되는 이벤트 ex) read

![[Pasted image 20250922151620.png]]

gateway
- exclusive: or
- parallel: and
- inclusive: or 이거나 and 이거나 상관 없음
- complex: 한번에 여러 개 이벤트

data

pool 과 pool 사이에는 메세지를 통해서 통신