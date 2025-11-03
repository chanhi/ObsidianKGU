
## System sofrware

![[Pasted image 20251021112638.png]]


### SIC, SIC/XE
SIC(Simplified Instructional Computer) / XE(eXtra Equipment)
SIC는 가상머신으로, 실제 머신을 사용하면 매우 복잡하기 때문에 쉽게 머신을 이해하기 위해서 일반적이지 않고 관련 없는 복잡함을 제거했다. SIC/XE는 SIC의 확장된 버전이다.

#### SIC Machine Architecture
Memory
- 8-bit bytes
- 3개의 연속적인 byte들로 word(24bits)를 형성
- 메모리 크기: 32KB(2^15 byte)


Registers
총 5개의 레지스터로 구성, 각 길이는 24bit
- A(0): 산술 명령어를 위해 사용
- X(1): 주소 지정을 위해 사용
- L(2): 서브루틴의 리턴 주소를 저장하는데 사용
- PC(8): Program Counter, 다음에 수행될 명령어의 주소를 저장
- SW(9): Status Word, 상태 코드 등을 포함하는 다양한 정보 저장


Data Formats
- 정수: 24-bit (음수: 2의 보수)
- 문자: 8-bit (ASCII)


명령어 포맷(Instruction format)
- 24-bit 포맷
|| opcode(8-bits) || X(1-bit) || address(15-bits) ||
- opcode(8비트): 명령어
- X(1비트): flag bit, Indexed addressing or Direct addressing mode
- address(15비트): 주소를 나타냄


Addressing Mode(주소지정 모드)

| mode    | indication | target address calculation         |
| ------- | ---------- | ---------------------------------- |
| Direct  | X=0        | Target Address, TA = Address       |
| Indexed | X=1        | Target Address, TA = address + (X) |
direct는 주소를 직접 사용, indexed는 해당 주소에 index 레지스터의 값을 더한 것을 사용



명령어 집합
- SIC에서는 일련의 기본적인 머신언어 명령어 집합을 제공
- A <- (A) + (m .. m+2) : A 레지스터에 A 레지스터 값과 메모리 주소 m~m+2에 있는 값(3byte owrd이므로)을 더해서 넣음
- () -> 해당 레지스터 값
Load & Store
- LDA m : A <- (m .. m+2)
- LDX m : A <- (m .. m+2)
- STA m : (m .. m+2) <- A
- STX m : (m .. m+2) <- A
- LDCH m : A[rightmost byte] <-
- STCH m : m <- (A)[rightmost byte]
Arithmetic & Logic
- ADD m : A <- (A) + (m .. m+2)
- SUB m : A <- (A) - (m .. m+2)
- MUL m : A <- (A) ** (m .. m+2)
- DIV m : A <- (A) / (m .. m+2)
- AND m : A <- (A) & (m .. m+2)
- OR m : A <- (A) | (m .. m+2)
Comparison
- COMP m : 비교되어 나온 값(크다, 작다, 같다)을 SW 레지스터의 Condition code에 저장
- TIX m : X <- (X) + 1; 후 X와 (m .. m+2)를 비교한 결과를 SW 레지스터의 Condition code저장
Conditional Jumps
- J m : PC <- (m .. m+2)
- JLT m : PC <- (m .. m+2) if CC set to <
- JGT m : PC <- (m .. m+2) if CC set to >
- JEQ m : PC <- (m .. m+2) if CC set to =
Subroutine Linkage
- JSUB m : L <- (PC); PC <- m
- RSUB : PC <- (L)  //리턴과 비슷
- RD m : A[rightmost byte] <- data from device specified by (nm)
- WD m : Device specified by (m) <- (A)[rightmost byte]


#### SIC/XE Machine Architecture
Memory
- 8-bit bytes
- 3개의 연속적인 byte들로 word(24bits)를 형성
- 메모리 크기: 1MB(2^20 byte)


Registers
SIC의 5개의 레지스터에 4개의 레지스터 추가, 각 길이는 24bit(F만 48bit)
- B(3): Base register, 어드레싱을 위해 사용
- S(4): General working register - 특정 목적은 없음
- T(5): General working register - 특정 목적은 없음
- F(6): Floating point accumulator, 실수 연산 레지스터(48 bits)


Data Formats
- 실수: 48-bits, 부동소수점
|s(1-bit)|exponent(11-bit)|fraction(36-bit)|
- 부호(1bit)(s=0 양수, s=1 음수)
- exponent(e) 11bit -> 정수 부분(0~2047)
- fraction(f) 36bit -> 소수 부분(0~1)
s x f x $2^{(e-1024)}$ 
2025.9 -> 0.20259 x $10^4$ -> e-1024 = 4 => e=1028
0.00020259 -> 0.20259 x $10^-{-3}$ -> e=1021


명령어 포맷(Instruction format)
- Format1 (1-바이트): opcode(8bit)
- Format2 (2-바이트): opcode(8bit) | r1(4bit) | r2(4bit)
- Format3 (3-바이트): opcode(8bit) | n | i | x | b | p | e (각 1bit, e=0) | displacement(12bit)
- Format4 (4-바이트): opcode(8bit) | n | i | x | b | p | e (각 1bit, e=1) | address(20bit) -> 48bit로 확장됨


Addressing Mode(주소지정 모드)
- relative addressing (format3에 적용)
	- b=1, p=0: base relative, Target Address, TA = (B) + disp ( 0 <= disp <= 4095 )
	- b=0, p=1: Program-Counter relative, Target Address, TA = (PC) + disp ( -2048 <= disp <= 2047 )
	- b: Base, p: PC로 암기
	- b=p=1 -> simple
	- disp는 위에 두 모드에 따라 이항 시키면 계산 가능
- Direct Addressing: b = 0 , p = 0 (format3 & format4)
	- format3의 disp 자체 값이 TA, format4 direct addressing 만이 허용
- Relative와 Direct Addressing 모드들은 기본적으로 Indexed Addressing(x = 1)과 함께 적용될 수 있다.
- Immediate Addressing: i = 1 and n = 0 (immediate의 i로 암기)
	- 메모리 참조 없이 TA가 operand 값으로 사용
- Indirect Addressing: i = 0 and n = 1
	- 메모리 참조 값으로 다시 메모리 접근
- Simple Addressing: (i = 0 and n = 0) or (i = 1 and n = 1)

- #: immediate addressing mode 
- X: indexed addressing mode 
- +: Format-4 Indicator 
- @: Indirect addressing mod

---

LDA 21 -> 030015
LDA #21 -> 010015
LDA @21 -> 020015

nixbpe  -> assembler language | TA
simple
110000 -> op c | disp
110001 -> +op m | addr (format4)
110010 -> op m | (PC) + disp
110100 -> op m | (B) + disp
111000 -> op c,X | disp + (X)
111001 -> +op m,X | addr, (X)
111010 -> op m,X | (PC) + disp + (X)
111100 -> op m,X | (B) + disp + (X)
000--- -> op m | bpe/disp
001--- -> op m,X | bpe/disp + (X)
indirect
100000 -> op @c | disp
100001 -> +op @m | addr
100010 -> op @m | (PC) + disp
100100 -> op@m | (B) + disp
immediate
010000 -> op \#c | disp
010001 -> +op \#m | addr
010010 -> op \#m | (PC) + disp
010100 -> op \#m | (B) + disp

0000/00 11/0010/ 0110/ 0000 0000    LDA var1
-> simple, relative(PC) |HEX = 032600 | TA = (PC) + disp = 3000 + 600 = 3600
0000/00 11/1100/ 0011/ 0000 0000     LDA var1, x
-> simple, relative(Base), indexed |HEX = 03C300 | TA = (B) + disp + (X) = 6000 + 300 + 0090 = 6390
0000/00 10/0010/ 0000/ 0011 0000    LDA @var1
-> indirect, relative(PC) |HEX = 022030 | TA = (PC) + disp = 3000 + 30 = 3030
0000/00 01/0000/ 0000/ 0011 0000    LDA \#x'30'
-> immediate |HEX = 010030 | LDA #30 = 30
0000/00 00/0011/ 0110/ 0000 0000
-> SIC |HEX = 00 3600 | TA = 3600
0000/00 11/0001/ 0000/ 1100 0011 0000 0011    +LDA var1
-> simple, format4 |HEX = 0310 C303 | TA = C303

LDA FIVE(FIVE의 주소가 000F, 인덱스 X 사용 안 함)
1. opcode: LDA -> 00
2. x 플래그 결정 -> x=0
3. address 변환 -> 000F
4. 00000F


----
#### Assembler
Assembler: SIC, SIC/XE와 같은 머신의 어셈블리 언어 프로그램을 해당 머신이 이해할 수 있는 목적 코드(Object Code)로 번역하는 시스템 소프트웨어
- 명령 코드를 기계어로 변환 (LDA -> 00)
- 명령 대상 변수를 동일한 기계 주소로 변환
- 적당한 명령어 포맷에 맞게 기계 명령어를 조립
- 명령 대상 상수를 기계 내부 명령어 포맷에 맞게 변환
- 목적 프로그램과 어셈블리 언어 리스트를 작성

Two-Pass 방식
Pass 1(심볼 정의)
- 모든 명령문에 주소를 할당
- 모든 레이블의 주소를 저장하여 Pass2에 사용
Pass 2(목적 코드 생성)
- 명령어를 조립
- BYTE, WORD 등으로 정의된 데이터 값을 생성
- 원시 프로그램을 목적 프로그램과 어셈블리 리스트 작성
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

원시 프로그램을 목적코드로 변환
1. 기억하기 쉬운 명령코드를 동일기능의 기계어로 변환
2. 명령 대상변수를 동일한 기계주소로 변환
3. 적당한 명령어 포맷에 맞게 기계명령어 조립
4. 명령 대상상수를 기계내부 병령어 포맷에 맞게 변환
5. 목적프로그램과 어셈블리언어 리스트 작성

SIC 목적 프로그램(Object program) 포맷
- Header record: 목적 프로그램 시작 위치와 목적 프로그램 크기
	- COL. 1: H
	- COL. 2-7: Program 이름
	- COL. 8-13: object program의 시작 주소 (hexadecimal)
	- COL. 14-19: object program 의 길이 in bytes (hexadecimal)
- Text record: 실제 코드
	- COL. 1: T
	- COL. 2-7: Starting address for object code in this record (hexadecimal)
	- COL. 8-9: Length of object code in this record in bytes (hexadecimal)
	- COL. 10-69: Object code, represented in hexadecimal (2 columns per byte of object code)
- End record: 실제 실행되는 목적 프로그램 주소
	- COL. 1: E
	- COL. 2-7: object program의 처음 실행된 주소 (hexadecimal)


SIC Machine Language Program Structure
Line numbers: 참고용이고 실제 프로그램을 구성하는 요소는 아님 
Source statements: Mnemonic instructions in Appendix A 
Addressing mode (주소지정방식): X 
Comments: “.”으로 시작하는 line들은 comments 처리함 
Machine directives (머신 지시어): 
- START 프로그램의 이름과 시작주소를 명시함 
- END 프로그램의 끝을 나타내며, 선택적으로 프로그램의 첫번째 실행명령어 위치를 명시함 
- BYTE 문자 또는 16진수 상수를 생성하며, 상수가 차지하는 바이트 수를 명시함 
- WORD 1-워드 정수의 상수를 생성하며, 상수가 차지하는 워드 수를 명시함 
- RESB 데이터 영역(변수)을 위해 명시된 바이트 수 만큼 메모리를 확보함 (1 x 명시된 값)
- RESW 데이터 영역(변수)을 위해 명시된 워드 수 만큼 메모리를 확보함 (3 x 명시된 값)

#### data structures for assembler
1 The Operational Code Table (OPTAB)

| Mnemonic Operation | Format | Opcode | Effect              |
| ------------------ | ------ | ------ | ------------------- |
| ADD m              | 3/4    | 18     | A <= (A) + (m..m+2) |

2 The Symbol Table (SYMTAB)

| Label Name | Location(Address) | Error Flag | Data Type |
| ---------- | ----------------- | ---------- | --------- |
| EOF        | 102A              | 0          | BYTE      |

3 The Location Counter (LOCCTR)


---
Example of a SIC assembler language program

| LINE | Location |             |          |            |                      |
| ---- | -------- | ----------- | -------- | ---------- | -------------------- |
| 5    | 1000     | COPY        | START    | 1000       | INPUT을 OUTPUT으로 copy |
| 10   | 1000     | FIRST       | STL      | RETADR     | RETURN ADDRESS 저장    |
| 15   | 1003     | CLOOP       | JSUB     | RDREC      | INPUT RECORD 읽음 루프   |
| 20   | 1006     |             | LDA      | LENGTH     | EOF인지 확인             |
| 25   | 1009     |             | COMP     | ZERO       | .                    |
| 30   | 100C     |             | JEQ      | ENDIL      | EOF면 루프 탈출           |
| 35   | 100F     |             | JSUB     | WRREC      | Write output으로 점프    |
| 40   | 1012     |             | J        | CLOOP      | 다시 루프                |
| 45   | 1015     | ENDFIL      | LDA      | EOF        | EOF 삽입               |
| 50   | 1018     |             | STA      | BUFFER     | .                    |
| 55   | 101B     |             | LDA      | THREE      | LENGHT = 3           |
| 60   | 101E     |             | STA      | LENGTH     | .                    |
| 65   | 1021     |             | JSUB     | WRREC      | Write EOF            |
| 70   | 1024     |             | LDL      | RETADR     | Retrun address 획득    |
| 75   | 1027     |             | RSUB     | C’EOF’     | caller에게 리턴          |
| 80   | 102A     | EOF         | BYTE     | J          |                      |
| 85   | 102D     | THREE       | WORD     | 3          |                      |
| 90   | 1030     | ZERO        | WORD     | 0          |                      |
| 95   | 1033     | RETADR      | RESW     | 1          |                      |
| 100  | 1036     | LENGTH      | RESW     | 1          | RECORD의 길이           |
| 105  | 1039     | BUFFER      | RESB     | 4096(1000) | 4096바이트              |
| 110  |          | .           |          |            |                      |
| 115  |          | .SUBROUTINE | TO READ  | RECORD     | INTO BUFFER          |
| 120  |          | .           |          |            |                      |
| 125  | 2039     | RDREC       | LDX      | ZERO       | clear loop counter   |
| 130  | 203C     |             | LDA      | ZERO       | clear A to ZERO      |
| 135  | 203F     | RLOOP       | TD       | INPUT      | input device 테스트     |
| 140  | 2042     |             | JEQ      | RLOOP      | Loop 준비              |
| 145  | 2045     |             | RD       | INPUT      | Character를 레지스터에 읽음  |
| 150  | 2048     |             | COMP     | ZERO       | A                    |
| 155  | 204B     |             | JEQ      | EXIT       | RECORD가 끝났는지 테스트     |
| 160  | 204E     |             | STCH     | BUFFER,X   | (X'00')              |
| 165  | 2051     |             | TIX      | MAXLEN     | 루프 탈출                |
| 170  | 2054     |             | JLT      | RLOOP      | 버퍼에 Character 저장     |
| 175  | 2057     | EXIT        | STX      | LENGTH     | 최대 길이까지 반복           |
| 180  | 205A     |             | RSUB     |            | .                    |
| 185  | 205D     | INPUT       | BYTE     | X’F1’      | RECORD LENGTH 저장     |
| 190  | 205E     | MAXLEN      | WORD     | 4096       | caller에게 리턴          |
| 195  |          | .           |          |            |                      |
| 200  |          | .SUBROUTINE | TO WRITE | RECORD     | FROM BUFFER          |
| 205  |          | .           |          |            |                      |
| 210  | 2061     | WRREC       | LDX      | ZERO       | Buffer로부터 RECORD 기록  |
| 215  | 2064     | WLOOP       | TD       | OUTPUT     |                      |
| 220  | 2067     |             | JEQ      | WLOOP      | Clear Loop Counter   |
| 225  | 206A     |             | LDCH     | BUFFER,X   | test output device   |
| 230  | 206D     |             | WD       | OUTPUT     | ready될 때까지 루프        |
| 235  | 2070     |             | TIX      | LENGTH     | 버퍼로부터 Chracter get   |
| 240  | 2073     |             | JLT      | WLOOP      | write Character      |
| 245  | 2076     |             | RSUB     |            | 모든 Charater가 쓰일 때까지  |
| 250  | 2079     | OUTPUT      | BYTE     | X’05’      | 루프                   |
| 255  |          |             | END      | FIRST      | caller에게 return      |

EXAMPLE: SIC PROGRAM WITH OBJECT CODE
COMP(28) ZERO(1030) -> 00101000 0 001 0000 0011 0000

| LINE | Location |             |          |            | OBJECT CODE |
| ---- | -------- | ----------- | -------- | ---------- | ----------- |
| 5    | 1000     | COPY        | START    | 1000       |             |
| 10   | 1000     | FIRST       | STL      | RETADR     | 141033      |
| 15   | 1003     | CLOOP       | JSUB     | RDREC      | 482039      |
| 20   | 1006     |             | LDA      | LENGTH     | 001036      |
| 25   | 1009     |             | COMP     | ZERO       | 281030      |
| 30   | 100C     |             | JEQ      | ENDIL      | 301015      |
| 35   | 100F     |             | JSUB     | WRREC      | 482061      |
| 40   | 1012     |             | J        | CLOOP      | 3C1003      |
| 45   | 1015     | ENDFIL      | LDA      | EOF        | 00102A      |
| 50   | 1018     |             | STA      | BUFFER     | 0C1039      |
| 55   | 101B     |             | LDA      | THREE      | 00102D      |
| 60   | 101E     |             | STA      | LENGTH     | 0C1036      |
| 65   | 1021     |             | JSUB     | WRREC      | 482061      |
| 70   | 1024     |             | LDL      | RETADR     | 081033      |
| 75   | 1027     |             | RSUB     | C’EOF’     | 4C0000      |
| 80   | 102A     | EOF         | BYTE     | J          | 454F46      |
| 85   | 102D     | THREE       | WORD     | 3          | 000003      |
| 90   | 1030     | ZERO        | WORD     | 0          | 000000      |
| 95   | 1033     | RETADR      | RESW     | 1          |             |
| 100  | 1036     | LENGTH      | RESW     | 1          |             |
| 105  | 1039     | BUFFER      | RESB     | 4096(1000) |             |
| 110  |          | .           |          |            |             |
| 115  |          | .SUBROUTINE | TO READ  | RECORD     | INTO BUFFER |
| 120  |          | .           |          |            |             |
| 125  | 2039     | RDREC       | LDX      | ZERO       | 040130      |
| 130  | 203C     |             | LDA      | ZERO       | 001030      |
| 135  | 203F     | RLOOP       | TD       | INPUT      | E0205D      |
| 140  | 2042     |             | JEQ      | RLOOP      | 30203F      |
| 145  | 2045     |             | RD       | INPUT      | D8205D      |
| 150  | 2048     |             | COMP     | ZERO       | 281030      |
| 155  | 204B     |             | JEQ      | EXIT       | 302057      |
| 160  | 204E     |             | STCH     | BUFFER,X   | 549039      |
| 165  | 2051     |             | TIX      | MAXLEN     | 2C205E      |
| 170  | 2054     |             | JLT      | RLOOP      | 38203F      |
| 175  | 2057     | EXIT        | STX      | LENGTH     | 101036      |
| 180  | 205A     |             | RSUB     |            | 4C0000      |
| 185  | 205D     | INPUT       | BYTE     | X’F1’      | F1          |
| 190  | 205E     | MAXLEN      | WORD     | 4096       | 001000      |
| 195  |          | .           |          |            |             |
| 200  |          | .SUBROUTINE | TO WRITE | RECORD     | FROM BUFFER |
| 205  |          | .           |          |            |             |
| 210  | 2061     | WRREC       | LDX      | ZERO       | 041030      |
| 215  | 2064     | WLOOP       | TD       | OUTPUT     | E02079      |
| 220  | 2067     |             | JEQ      | WLOOP      | 302064      |
| 225  | 206A     |             | LDCH     | BUFFER,X   | 509039      |
| 230  | 206D     |             | WD       | OUTPUT     | DC2079      |
| 235  | 2070     |             | TIX      | LENGTH     | 2C1036      |
| 240  | 2073     |             | JLT      | WLOOP      | 382064      |
| 245  | 2076     |             | RSUB     |            | 4C0000      |
| 250  | 2079     | OUTPUT      | BYTE     | X’05’      | 05          |
| 255  |          |             | END      | FIRST      |             |


