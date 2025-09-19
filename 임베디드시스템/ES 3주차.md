
interrupt: CPU와 소통하기 위한 방법

interrupt controller가 어떤 장치가 interrupt를 발생시켰는지 파악
interrupt number
interrupt 종류
- maskable interrupt: enable/disable이 가능, 무시 가능
- nonmaskable interrupt: reset 과 같이 무시 불가능

ISR (Interrupt Service Routine): 인터럽트를 실제로 처리하는 S/W
- block되면 안됨
- top-half: H/W에 관계된, 최대한 빨리 처리해야 되는 작업, 즉 먼저 처리
- bottom-half: ISR에서 처리할 필요가 없고, 시간이 오래 걸리는 작업
- Context save & restore: INT -> ISR -> 원래 프로그램 => 중간에 정보를 저장, 로드할 필요가 있음

IVT (Interrupt Vector Table)
- Interrupt와 해당 Interrupt의 ISR과의 맵핑 제공(추가적인 메모리 필요)
- IVT는 H/W의 특정 주소에 위치(ARM 프로세서의 경우 IVT는 0x00에서부터 한 word씩 할당)


ARM Processor
7개의 동작모드 정의
- User
- FIQ: 고속 인터럽트 처리
- IRQ: 일반 인터럽트 처리
- Abort
- Undefined
- System

레지스터
- ARM은 37개의 32 비트 길이의 레지스터 포함
	 범용 레지스터 (30), PC (1), 상태 레지스터 (6)
- CPSR (Current Program Status Register)‒ 현재 동작중인 프로세서의 상태 저장‒ 모든 동작 모드에서 공유  SPSR (Saved Program Status Register)‒ 모드 전환 시 이전 동작 모드의 CPSR 복사해서 보관‒ SVC/Abort/Undefined/IRQ/FIQ를 위한 5개의 SPSR 존재  SP (Stack Pointer)‒ 프로그램에서 사용하는 메모리 중 Stack의 TOP 위치 저장‒ 동작 모드마다 별도의 SP 존재  LR (Link Register)‒ Subroutine 함수로 되돌아갈 주소를 저장하거나 예외 처리 후 되돌아갈 주소 정보 저장  범용 레지스터의 경우 별도 백업 레지스터를 사용하지 않고 Stack 사용


Program counter(PC)에 백터 데이터 

Interrupt Latency: Interrupt가 발생했을 때부터 프로세서가 관련 ISR을 수행하기 시작할 때까지 걸린 시간

네트워크 하드웨어
의 latency(우선순위가 최대일 때): 최소 0, 최대 300(IPC)


임베디드 시스템 설계 절차

개발 환경 설정
자원이 풍부한 컴퓨팅 환경(host system)에서 개발한 뒤 결과물을 임베디드 시스템(target system)에 삽입해서 수행 => 교차 개발 환경
cross compiler: Host System에서 개발된 프로그램을 Target System의 기계어로 변환하는 Compiler
native compiler: 해당 pc 컴파일러