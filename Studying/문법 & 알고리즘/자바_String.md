---
keyword : Java, String
class : Grammer
---

### Java의 String 
- String 클래스 = char[] + 메서드(기능)
- String 클래스는 내용을 변경할 수 없다.(read only)
	- String a = "a";
	- String b = "b"
	- a = a + b; > "ab"라는 새로운 문자열이 생성됨
- 주요 메서드
	- Char charAt(int index) // String에서 character 접근
		- String str = "abcde"
		- char ch = str.charAt(3);
		- 문자열 d를 ch에 저장
	- str.length()
	- substring
		- String str = "012345"
		- String tmp = str.substring(1, 4);
		- 1번 인덱스부터 4 이전(3) 인덱스까지 반환 > 123 반환// String 자르기
	- equals
	- char[] chars = string.toCharArray;  //  문자열을 하나씩 쪼개어 char 배열로 만들어줌
	- String str = new String(charArr); //  문자배열을 문자열로
	- Character.toUpperCase();  // 문자를 대문자로



#### String.valueOf();
  - 파라미터가 Null 이면 Null 값을 반환
#### toString();
  - Null값이 있으면 NullPointException 발생'
