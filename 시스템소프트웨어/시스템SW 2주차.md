
----
![[Pasted image 20250910153850.png]]
각 파일 확장자 의미, C library에는 .a 아카이브 파일로 존재, 합쳐져서 실행파일 생성

파스칼 파일 -> 파스칼 컴파일러 -> SIC/XE 어셈블리 프로그램 -> SIC/XE 어셈블러 -> 파스칼 object 파일 -> 링커 -> 파스칼 실행파일 -> 로더

```c
#include <stdio.h>

int main(void){
	int ALPHA;
	int FIVE = 5;
	char C1;
	char CHARZ = 'Z';
	
	ALPHA = FIVE;
	C1 = CHARZ;
	
	return 0;
}
```
![[Pasted image 20250910155243.png]]
어셈블러 통과 후 기계어로 변환


