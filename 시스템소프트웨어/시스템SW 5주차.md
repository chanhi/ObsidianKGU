
SIC Assembler

원시 프로그램을 목적코드로 변환

1. 기억하기 쉬운 명령코드를 동일기능의 기계어로 변환
2. 명령 대상변수를 동일한 기계주소로 변환
3. 적당한 명령어 포맷에 맞게 기계명령어 조립
4. 명령 대상상수를 기계내부 병령어 포맷에 맞게 변환
5. 목적프로그램과 어셈블리언어 리스트 작성
목적 프로그램(Object program) 포맷
- Header record: 목적 프로그램 시작 위치와 목적 프로그램 크기
	- COL. 1: H
	- COL. 2-7: Program Name
	- COL. 8-13: Starting address of object program (hexadecimal)
	- COL. 14-19: Length of object program in bytes (hexadecimal)
- Text record: 실제 코드
	- COL. 1: T
	- COL. 2-7: Starting address for object code in this record (hexadecimal)
	- COL. 8-9: Length of object code in this record in bytes (hexadecimal)
	- COL. 10-69: Object code, represented in hexadecimal (2 columns per byte of object code)
- End record: 실제 실행되는 목적 프로그램 주소
	- COL. 1: E
	- COL. 2-7: Address of first executable instruction in object program (hexadecimal)

H^COPY ^001000^00107A T^001000^1E^141033^482039^001036^281030^301015^482061^3C1003^00102A^0C1039^00102D T^00101E^15^0C1036^482061^081033^4C0000^454F46^000003^000000 T^002039^1E^040130^001030^E0205D^30203F^D8205D^281030^302057^549039^2C205E^38203F T^002057^1C^101036^4C0000^F1^001000^041030^E02079^302064^509039^DC2079^2C1036 T^002073^07^382064^4C0000^05 
E^001000


TWO-PASS ASSEMBLER's Functions
- Pass 1(define symbols)
	1. 모든 명령문에 주소를 할당함
	2. Pass 2에서 사용할 수 있도록 모든 label에 할당된 주소를 저장함
	3. 어셈블러 지시어를 처리함
- Pass 2(assemble instructions and generate object program)
	1. 명령어를 조립함
	2. 데이터 포맷 지시어에 따른 데이터 값을 생성함
	3. Pass 1동안에 처리가 안된 어셈블러 지시어를 처리함
	4. 목적 프로그램과 어셈블리 리스트를 작성함

data structures for assembler
The Operational Code Table (OPTAB)

| Mnemonic Operation | Format | Opcode | Effect              |
| ------------------ | ------ | ------ | ------------------- |
| ADD m              | 3/4    | 18     | A <= (A) + (m..m+2) |

The Symbol Table (SYMTAB)

| Label Name | Location(Address) | Error Flag | Data Type |
| ---------- | ----------------- | ---------- | --------- |
| EOF        | 102A              | 0          | BYTE      |

The Location Counter (LOCCTR)

assembler design
![[Pasted image 20251006200242.png]]

