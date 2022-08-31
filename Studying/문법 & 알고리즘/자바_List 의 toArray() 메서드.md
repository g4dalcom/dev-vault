---
keyword : Java
class : Grammer
---


### JAVA List 의 toArray() 메서드

 List 컨테이너의 인스턴스를 배열(array)로 만드는것이 'toArray' 메서드이다. 하지만 이 메서드 사용에 있어서 아래와같은 아리송한 코드를 자주 봤을 것이다.  

```java
List<String> stringList = new ArrayList<String>();
stringList.add("A");
stringList.add("B");
stringList.add("C");
        
String[] stringArray = **stringList.toArray(new String[0])**;
```

메서드 이름에서 직관적으로 알 수 있듯이, List를 Array로 바꿔주는 메서드인데, 파라메터로 들어가는 인자가 이상하다.  
  
위의 예제에서는 String 배열 인스턴스가 파라메터로 넘어갔는데, size를 '0'으로 명시했다.  
이것이 무슨의미일까? 정리하자면 아래와 같다.  
  
**1. List를 toArray 메서드에 파라메터로 넘어가는 배열 객체의 size만큼의 배열로 전환한다.**  
**2. 단, 해당 List size가 인자로 넘어가는 배열 객체의 size보다 클때, 해당 List의 size로 배열이 만들어진다.**  
**3. 반대로 해당 List size가 인자로 넘어가는 배열객체의 size보다 작을때는, 인자로 넘어가는 배열객체의 size로 배열이 만들어진다.**  
  
여기서 위의 예제에서의 stringArray의 크기는 '3' 이다. 인자로 넘어가는 배열의 size가 '0'이므로, 원래 List의 size로 배열이 만들어진 것이다.  
  
하지만 아래와 같은 예제라면 stringArray의 크기가 얼마가 될까?  

```java
List<String> stringList = new ArrayList<String>();
stringList.add("A");
stringList.add("B");
stringList.add("C");
        
String[] stringArray = stringList.toArray(new String[5]);

System.out.println("array size : " + stringArray.length);
```

출력 결과는 아래와 같다  

array size : 5

뭐, 당연한 결과다. toArray 메서드의 인자로 넘어가는 배열 size가 원래 List사이즈보다 크므로, 인자로 넘어가는 배열 size로 배열이 만들어진다.  



[출처](http://asuraiv.blogspot.com/2015/06/java-list-toarray.html)
