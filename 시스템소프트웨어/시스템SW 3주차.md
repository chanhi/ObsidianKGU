MACHNINE LANGUAGE
SIC/XE
![[Pasted image 20250917142434.png]]

c파일 -> C compiler -> ML프로그램(SIC/XE코드) -> 어셈블러(SIC/XE) -> 목적파일(바이너리)

응용 소프트웨어: 컴퓨팅 머신을 도구로 하여 주어진 문제의 해결책을 모색하는데 집중
시스템 소프트웨어: 컴퓨팅 머신 자체의 운용과 사용을 지원하는데 집중

SIC and SIC/XE (Simplified Instructional Computer / EXTENSION)
- Not a real computer But a hypothetical computer
- 머신의 기본적인 기능을 쉽게 이해시키기 위한 단순화된 컴퓨팅 머신
- 실제 머신 아키텍처의 하드웨어 기능적 복잡성 최소화한 머신 아키텍처
- 총 5개의 레지스터로 구성되며, 각 레지스터의 길이는 24비트

SIC
- 8비트 바이트로 구성
- 3개의 연속적인 바이트들로 워드를 형성
- 메모리 주소는 바이트 단위의 주소 배정
- Word의 주소는 해당 바이트들 중에서 가장 낮은 주소로 지정

명령어 포맷(24bit): opcode(8bit), mode(1bit), address(15bit)
데이터 포멧: 정수(24비트 2진수, 음수 2의보수), 문자 8비트 ASCII 코드


SIC/XE
- 8bit 바이트

데이터 포맷
실수: 48비트
- 부호(1bit)
- exponent(e) 11bit
- fraction(f) 36bit

f x $2^{e-1024}$ 
2025.9 -> 0.20259 x $10^4$ -> e-1024 = 4 => e=1028
0.00020259 -> 0.20259 x $10^-{-3}$ -> e=1021

명령어 포맷
1. 1-바이트: opcode(8bit)
2. 2-바이트: opcode(8bit), r1(4bit), r2(4bit)
3. 3-바이트: opcode(8bit), n/i/x/b/p/e(각 1bit) ,displacement(12bit, 기준(PC)으로부터 거리)-> 상대 주소
4. 4-바이트: opcode(8bit), n/i/x/b/p/e(각 1bit) ,address(20bit) -> 절대 주소

주소지정 모드
- relative addressing
	- b=1, p=0: base relative, Target Address, TA = (B) + disp ( 0 <= disp <= 4095 )
	- b=0, p=1: Program-Counter relative, Target Address, TA = (PC) + disp ( -2048 <= disp <= 2047 )
- Direct Addressing: b = 0 , p = 0
- Relative와 Direct Addressing 모드들은 기본적으로 Indexed Addressing(x = 1)과 함께 적용될 수 있다.
- Immediate Addressing: i = 1 and n = 0
- Indirect Addressing: i = 0 and n = 1
- Simple Addressing: (i = 0 and n = 0) or (i = 1 and n = 1)

- #: immediate addressing mode 
- X: indexed addressing mode 
- +: Format-4 Indicator 
- @: Indirect addressing mod

(시작이 0일 때) pc=3(다음에 실행할 opcode 위치)



B, KB, MB, GB, TB, PB, EB, ZB, YB
2^0 2^10 2^20 2^30
요타를 시험에 내겠다고...?

