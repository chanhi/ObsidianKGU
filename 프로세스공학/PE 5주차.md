
XML & XPDL


XML(eXtensible Markup Language)
데이터 자체를 표현하는 문서

```XML
<note>
	<to>chan</to>
	<from>huy</from>
	<head>note</head>
	<body>note body</body>
</note>
```
어떠한 기능을 하는 것이 아니라 그저 데이터 자체를 보여주는 문서

모든 것을 단순화시켜야 한다.
- 데이터 공유, 전송, 플랫폼 변화 등을 단순화

종종 HTML과 상호 보완한다.
HTML로부터 데이터를 분리한다.

show.html <->data.xml

같은 데이터라도 수많은 xml 포맷이 존재한다.

XML Tree
xml 트리 구조: root 부터 leaf까지 보통의 트리 구조

문법 구조
- 반드시 root, 즉 부모 element가 있어야 함
- xml 프롤로그는 option, 쓴다면 첫 줄(밑 예제 첫 줄)
- 클로징 태그 항상 필요, 오픈 태그와 항상 같아야 됨(properly nesting)
  ```XML
	<?xml version="1.0" encoding="UTF-8"?>
	<root>
		<child>
			...
		</child>
	</root>
	<!-- 주석 -->
	```

&lt;  =  < 
&gt;  = >
&amp;  =  &
&apos;  =  '
&quot;  = "
- xml은 공백, 엔터 등을 값으로 침

attribute
```
<tag 속성명="값"> </tag>
```

이름이 충돌이 나면 안됨(루트 태그 이름)

XMLHttpRequest Object

XML Parser: 가져온 텍스트나 파일을 xml형태로 파싱하는 것
```
parser = new DOMParser();
xmlDoc = parser.parseFromString(text, "text/xml");
```