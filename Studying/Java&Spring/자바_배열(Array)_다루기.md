![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ftdkae%2Fbtsb2TBmE2S%2FzVRffbmAFXkY4MmmI4JGt1%2Fimg.png)

# 개요

- 배열은 프로그래밍이나 알고리즘을 풀 때 항상 사용하게 되는 자료형인데 사용할 때마다 새로운 것을 알게되는 것 같다. 
- 기초이자 너무나 중요한 요소이기 때문에 새로운 것을 알게될 때마다 업데이트를 하고 두고두고 찾아보고자 개념 정리를 해보기로 하였다!


## 배열 선언 & 초기화

- 배열을 선언할 때 미리 타입과 크기를 지정해주어야 한다.

```java
// int 형 배열 선언 & 초기화
int[] score = new int[5]; // int 타입의 값 5개가 저장될 빈 공간 생성
// 초기화
score[0] = 10;
score[1] = 20;
score[2] = 30;
score[3] = 40;
score[4] = 50;

// for문으로 배열을 순차적으로 순회에 값을 넣어주는 방법도 있다.
for(int i = 0; i < score.length; i++){
	score[i] = (i+1) * 10;
}

// 처음부터 선언 + 초기화를 한번에 진행
int[] score = {10, 20, 30, 40, 50};
```

## 배열 출력

- 배열의 내용을 확인하려고 **System.out.println**을 하게 되면 배열의 주소값이 출력된다.(char[] 은 예외!)

```java
int[] arr = {10, 20, 30, 40, 50};
char[] ch = {'a', 'b', 'c'};


// [I : 배열의 타입(Integer), @7ad041f3 : 주소값
System.out.println(arr); // [I@7ad041f3
System.out.println(ch);  // abc
```

- 따라서 for문으로 순회하여 출력해주거나 **Arrays.toString()** 메소드를 이용해서 배열을 문자열 형식으로 만들어 출력해야 한다.

```java
import java.util.Arrays; // Arrays.toString()을 사용하기 위한 import

int[] arr = {10, 20, 30, 40, 50};

// for문으로 배열의 원소 출력
for(int i = 0; i < arr.length; i++){
	System.out.println(arr[i]);
}

// Arrays.toString() 메소드 사용
System.out.println(Arrays.toString(arr));
```


## 배열 복사

- 배열을 선언할 때 타입과 크기를 지정해준다고 하였는데, 배열의 크기 이상으로 데이터를 저장해야 하는 상황이 생길 수도 있다.
- 이 때는 **공간이 큰 배열을 새로 만들고 기존의 배열의 내용을 새로 만든 배열에 복사**하는 식으로 간접적으로 확장해야 한다.
- 배열을 복사하는 방법은 for문으로 순회하며 일일이 복사하는 방법과 **System.arraycopy()**, **Arrays.copyOf()** 메소드를 이용하는 방법이 있다.

> Arrays.copyOf는 System.arraycopy를 래핑한 함수로 둘은 동일하다고 보아도 된다!


#### for문 순회

```java
int[] arr1 = {10, 20, 30, 40, 50};

// 전보다 크기가 두 배인 배열 선언
int[] arr2 = new int[arr1.length * 2]; 

for(int i = 0 ; i < arr1.length ; i++) { 
	arr2[i] = arr1[i];	
}
// 원래의 배열을 가리키고있던 참조변수 arr1이 새로 복사된 arr2 배열을 가리키도록 한다.
arr1 = arr2; 
```

#### System.arraycopy()

```java
 int[] arr1 = {10, 20, 30, 40, 50};

int[] arr2 = new int[arr1.length * 2];

// arr1의 index 0부터 arr1.length 전체 길이 만큼 arr2의 index 0부터 붙여넣는다.
System.arraycopy(arr1, 0, arr2, 0, arr1.length); 
/*
- 첫번째 인자 : 복사할 배열
- 두번째 인자 : 복사를 시작할 배열의 위치
- 세번째 인자 : 붙여넣을 배열
- 네번째 인자 : 복사된 배열값들이 붙여질 시작위치 (차례대로 붙여 넣어진다)
- 다섯번째 인자 : 지정된 길이만큼 값들이 복사된다.
*/
```


#### Arrays.copyOf(), Arrays.copyOfRange()

```java
int[] arr1 = {10, 20, 30, 40, 50};

int[] arr2 = new int[arr1.length * 2];

// arr1 배열을 arr1.length 전체 길이만큼 전체 복사해서 arr2에 할당
arr2 = Arrays.copyOf(arr1, arr1.length); 

// 시작점과 끝점을 지정
arr2 = Arrays.copyOfRange(arr1, 1, 3); 
System.out.println(Arrays.toString(arr2)); // [10, 20]
```

> for문보다 메소드를 이용하는 게 거의 두 배정도 성능이 빠르다고 한다!


## 배열 정렬

- **Arrays.sort()** 를 이용해 정렬할 수 있다.
- 이 때 새로운 배열이 반환되는 것이 아니라 원본 요소가 변하는 것이므로 원본 요소를 보존하려면 배열 복사를 한 뒤 복사한 배열을 정렬해주어야 한다!

```java
int[] arr = {3, 2, 0, 1, 4};

// 오름차순 정렬
Arrays.sort(arr);
System.out.println(Arrays.toString(arr)); // [0,1,2,3,4]

// 내림차순 정렬 
Arrays.sort(arr, Collections.reverseOrder()); // 배열을 내림차순으로 정렬할 때는 Collections 클래스의 reverseOrder() 함수를 사용
System.out.println(Arrays.toString(arr)); // [4,3,2,1,0]

// 배열 일부부만 정렬
int[] arr = { 3,2,0,1,4 };
Arrays.sort(arr, 0, 3); // 배열 요소 0, 1, 2 만 정렬
System.out.println(Arrays.toString(arr)); // [0, 2, 3, 1, 4]
```


## 배열 비교

- 두 배열의 요소가 같은 요소들로 이루어져 있는지를 비교하기 위해서는 for문으로 일일이 비교하는 방법과 **Arrays.equals()** 메소드를 이용하는 방법이 있다.

```java
String[] arr1 = {"치즈", "고등어", "삼색이", "카오스"};
String[] arr2 = {"치즈", "고등어", "삼색이", "카오스"};
String[] arr3 = {"치즈", "고등어", "삼색이"};

Arrays.equals(arr1, arr2); // true

Arrays.equals(arr1, arr3); // false
```


# 다차원 배열

- 2차원 이상의 배열이며 배열의 요소로 또 다른 배열을 가지는 배열을 의미한다. 주로 2차원 배열을 많이 사용하기 때문에 2차원 배열을 위주로 정리하고자 한다!

## 배열 생성 & 초기화

- 2차원 배열은 표, 엑셀 시트를 생각하면 된다.
- 테이블 형태의 데이터는 행(row)과 열(column)으루 구성되어 있고, 행(1차원 배열)이 여러 개 모인 것이 열(2차원 배열)이 되는 것이다.

|       | 과목1 | 과목2 | 과목3 |
|:----- |:----- |:----- |:----- |
| 학생1 | 90   | 80   | 100   |
| 학생2 | 70    | 100   | 95    |
| 학생3 | 50    | 80    | 90    |
| 학생4 | 80    | 60    | 100      |

- int[] 학생1 = {100, 100, 100}; 이라는 1차원 배열(row)이 여러개(column, 학생1, 학생2, 학생3, 학생4) 모이면 2차원 배열이 된다.

```java
int[][] score = new int[4][3];
score[0][0] = 90;
score[0][1] = 80;
score[0][2] = 100;
.
.
.

// 선언과 동시에 초기화
int[][] score = {
					{90, 80, 100},
					{70, 100, 95},
					{50, 80, 90},
					{80, 60, 100}
				}

```


## 2차원 배열 출력

- 1차원 배열을 출력할 때와 마찬가지로 for문으로 순회하여 출력하는 방법과 **Arrays.deepToString()** 메소드를 이용하는 방법이 있다.

```java
int[][] arr = {
                  {10,20,30},
                  {40,50,60},
                  {70,80,90},
                  {100,200,300}
                };

// for문 출력
for(int i = 0; i < arr.length; i++) {
    for(int j = 0; j < arr[i].length; j++) { 
        System.out.println(arr[i][j]);
    }
}

// Arrays.deepToString()
System.out.println(Arrays.deepToString(arr));
```


## 2차원 배열 비교

- 1차원 배열은 Arrays.equals()를 사용했지만 2차원 배열은 **Arrays.deepEquals()**  메소드로 비교할 수 있다.

```java
String[][] arr1 = { 
		            { "치즈", "고등어" },
		            { "삼색이", "카오스" }
			      };
String[][] arr2 = { 
		            { "치즈", "고등어" },
		            { "삼색이", "카오스" }
				  };
String[][] arr3 = { 
					{ "치즈", "고등어" },
					{ "턱시도", "얼룩이" }
				  };

Arrays.deepEquals(arr1, arr2); // true

Arrays.deepEquals(arr1, arr3); // false
```


# 객체 배열

- 프로그래밍을 하거나 고단계 알고리즘으로 갈 수록 객체 배열의 사용 빈도가 올라가는 것 같다.
- 원시형 타입 배열과 객체 배열의 가장 큰 차이는 배열 복사를 할 때인데, 원시형 타입은 값 자체를 복사하지만 객체는 참조 자료형으로 주소값이 복사되기 때문에 복사한 배열을 변경하면 원본까지 변경될 수가 있다. (얕은 복사)

```java
class myObject{
    int id;
    String description;

    myObject(int id, String description) {
        this.id = id;
        this.description = description;
    }
}

myObject[] arrayObj = {
    new myObject(101, "first"), 
    new myObject(101, "second"), 
    new myObject(101, "third")
};
System.out.println(Arrays.toString(arrayObj)); // [main$1myObject@251a69d7, main$1myObject@7344699f, main$1myObject@6b95977]

myObject[] arrayObj2; // 복사할 배열

arrayObj2 = arrayObj.clone(); // 배열을 복사해도 내용물 객체의 주소는 똑같다.
System.out.println(Arrays.toString(arrayObj2)); // [main$1myObject@251a69d7, main$1myObject@7344699f, main$1myObject@6b95977]

System.out.println(arrayObj[0].id); // 101
arrayObj2[0].id = 999; // 복사한 arrayObj2의 첫째 객체의 멤버를 변경
System.out.println(arrayObj2[0].id); // 999
System.out.println(arrayObj[0].id); // 999, 원본 객체의 멤버도 변경이 된다.
```

- 따라서 for문으로 깊은 복사를 해주어야 한다.

```java
myObject[] arrayObj2 = new myObject[3];
for(int i = 0; i < arrayObj.length; i++) {
    arrayObj2[i] = new myObject(arrayObj[i].id, arrayObj[i].description);
}
```


## 객체 배열 정렬

- 객체 배열 내 요소를 정렬하기 위해서는 정렬 기준이 필요하다. 이 때 사용하는 것은 Comparable과 Comparator 메소드이다. 
- 이 메소드들은 따로 정리를 해야할 정도로 양이 방대하고 내용 또한 까다로운데, 여기에서는 객체 배열을 정렬하기 위한 방법을 익히는 정도로만 정리해보고자 한다!

```java
class Member {
	String name;
	int age;

	Member(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

Member[] members = {
	new Member("풍월량", 40),
	new Member("침착맨", 39),
	new Member("량월풍", 25),
	new Member("풍꿀미", 22),
}
```

### Comparable

- 자기 자신과 매개변수 객체를 비교
- compareTo() 메소드를 오버라이딩하여 사용한다.
- lang 패키지에 속해있기 때문에 import가 필요하지 않다.

```java
// 클래스에 Comparable 인터페이스 구현
class Member implements Comparable<Member> {
	String name;
	int age;

	Member(String name, int age) {
		this.name = name;
		this.age = age;
	}

	// compareTo로 나이 비교
	@Override
	public int compareTo(Member member) {
		if (this.age < member.age) {
			return -1;
		} else if (this.age == member.age) {
			return 0;
		} else {
			return 1;
		}
	}
}

Member[] members = {
	new Member("풍월량", 40),
	new Member("량월풍", 25),
	new Member("침착맨", 39),
	new Member("풍꿀미", 22),
}

Arrays.sort(members);

for (Member m : members) {
	System.out.println(m.name + " " + m.age + "세");
}
```

```java
풍꿀미 22세
량월풍 25세
침착맨 39세
풍월량 40세
```


### Comparator

- 파라미터로 들어오는 두 매개변수 객체를 비교
- compare() 메소드를 오버라이딩해서 사용
- util 패키지에 속해있기 때문에 import가 필요하다.

```java
class Member {
	String name;
	int age;

	Member(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

Member[] members = {
	new Member("풍월량", 40),
	new Member("침착맨", 39),
	new Member("량월풍", 25),
	new Member("풍꿀미", 22),
}

Arrays.sort(members, new Comparator<Member>() {
	@Override
	public int compare(Member o1, Member o2) {
		// Integer 클래스에 정의된 compare 함수로 두 가격 정수 원시값을 비교
		return Integer.compare(o1.age, o2.age);
	}
});
```

```java
Arrays.sort(members, (o1, o2) -> Integer.compare(o1.age, o2.age));
```
- 정렬하는 부분을 위와 같이 람다식으로 축약도 가능하다!

- 만일 나이(int)가 아니라 이름(String)순으로 정렬을 하려면 compare() 대신 compareTo() 메소드를 사용해야 한다.

```java
Arrays.sort(members, new Comparator<Member>() {
	@Override
	public int compare(Member o1, Member o2) {
		return o1.name.compareTo(o2.name); 
	}
});

//람다식
Arrays.sort(members, (o1, o2) -> o1.name.compareTo(o2.name));
```

```java
량월풍 25세
침착맨 39세
풍꿀미 22세
풍월량 40세
```


## 다중 조건 비교(comparing / thenComparing)

- 만약 나이순으로 정렬을 하였는데 나이가 같은 사람이 존재할 경우 추가적으로 이름순으로 정렬을 해줄 필요가 있을 수 있다.
- 이처럼 객체의 여러 속성을 이용하여 정렬하기 위해서는 Comparator의 comparing()과 thenComparing()을 이용해 구현이 가능하다.
- 이 때 객체의 속성을 가져올 때 getter, setter를 이용해야 한다!

```java
class Member {
	String name;
	int age;

	Member(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public int getAge() {
		return this.age;
	}

	public String getName() {
		return this.name;
	}
}

Member[] members = {
	new Member("풍월량", 40),
	new Member("침착맨", 39),
	new Member("량월풍", 25),
	new Member("풍꿀미", 22),
	new Member("물개뇨속", 40),
}

// 먼저 나이순으로 정렬한 뒤 나이가 같다면 이름순으로 정렬
Arrays.sort(members, Comparator.comparing(Member::getAge).thenComparing(Member::getName));
```

```java
풍꿀미 22세
량월풍 25세
침착맨 39세
물개뇨속 40세
풍월량 40세
```


---

### 참고 자료

- [자바 \[JAVA\] - Comparable과 Comparator의 이해](https://st-lab.tistory.com/243)
- [나무소리 자바 기초 강의, 배열의 이해](https://www.youtube.com/watch?v=LKdGVsPPXQU&list=PLOSNUO27qFbtjCw-YHcmtfZAkE79HZSOO&index=32)
- [JAVA 배열(Array) 완벽 다루기 가이드](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94-%EB%B0%B0%EC%97%B4Array-%EB%AC%B8%EB%B2%95-%EC%9D%91%EC%9A%A9-%EC%B4%9D%EC%A0%95%EB%A6%AC)
