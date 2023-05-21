# 제네릭이란?!

제네릭은 **클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법**이다.

```java
ArrayList<Integer> list = new ArrayList<>();
```

주로 Collection Framwork를 선언할 때 **꺽쇠 괄호(<>)** 를 함께 사용하는데 그 꺽쇠 괄호가 제네릭이다.

괄호 안에 타입을 쓰면 해당 컬렉션의 자료형 타입이 지정되는데 위 예제의 경우 list에는 Integer 타입만 저장할 수 있게 된다.

제네릭에 대해 공부하기 전에는 변수에 타입을 지정하듯이 컬렉션에 타입을 지정하는 것이라고 추측하였었는데 맞는 말이긴 하지만 변수 타입보다 더 깊은 내용이 있다.

```java
String[] str = new String[5];

ArrayList<String> list = new ArrayList<>(5);
```


## 제네릭 타입 매개변수

#### 정의

제네릭을 이용한 클래스나 메소드를 설계할 때 제네릭 기호 안에 식별자를 지정함으로써 파라미터처럼 사용할 수 있다.

```java
// 타입 매개변수로 식별자 T를 선언
class Box<T> {
	List<T> items = new ArrayList<>();

	public void add(T item) {
		items.add(item);
	}
}
```

```java
Box<Integer> intBox = new Box<>();

Box<Apple> appleBox = new Box<>();

Box<Mango> mangoBox = new Box<>();
```

파라미터를 전달 받아서 사용하듯이 식별자 자리에 타입을 지정하면 해당 인스턴스의 모든 타입 매개변수가 지정된 타입으로 변환된다.

```java
class Box<Mango> {
	List<Mango> items = new ArrayList<>();

	public void add(Mango item) {
		items.add(item);
	}
}
```

> 타입 파라미터로 사용한 식별자 \<T\>는 아무 문자나 넣어도 상관이 없지만 아래와 같이 통상적으로 사용하는 네이밍이 있다는 점을 참고하자!

| 타입  | 설명            |
|:----- |:--------------- |
| \<E\> | Element         |
| \<K\> | Key             |
| \<V\> | Value, Variable |
| \<N\> | Number          |
| \<T\> | Type                |


#### 타입 파라미터 생략

new 생성자 부분에 제네릭 타입을 쓰지 않는 이유는 제네릭이 타입 추론을 통해 생략된 곳을 채워주기 때문이다.

```java
Box<Integer> intBox = new Box<Integer>();

// 타입추론으로 알아서 채워짐
Box<Integer> intBox = new Box<>();
```

> 참고로 제네릭으로 할당받을 수 있는 타입은 참조형 타입만 가능하다. 따라서 int, char형과 같은 원시형 타입은 Integer, Character와 같은 Wrapper 클래스로 변환해주어야 한다.

#### 복수 타입 파라미터

타입 지정이 여러개가 필요할 경우 복수 타입 파라미터를 지정할 수 있다.

```java
public class HashMap<K, V> {...}
```

해시맵을 선언할 때를 생각해보면 된다.

```java
HashMap<Integer, String> map = new HashMap<>();
```

복수로 선언된 만큼 제네릭 타입 또한 지정해주어야 한다.

#### 중첩 타입 파라미터

제네릭 객체를 제네릭 타입 파라미터로 받는 형식도 가능하다. 알고리즘 문제를 풀 때 많이 사용된다.

```java
ArrayList<ArrayList<Integer>> list = new ArrayList<>();

for (int i = 0; i < 3; i++) {
	list.add(new ArrayList<>());
}
// => [[], [], []]

for (int i = 0; i < 3; i++) {
	for (int j = 0; j <= i; j++) {
		list.get(i).add(j);
	}
}
// => [[0], [0, 1], [0, 1, 2]]
```


## 제네릭의 필요성

사과를 박스에 담아서 보관하려고 한다고 가정해보자!
아래와 같이 사과 클래스를 가져와서 사과 상자에 담고 필요할 때 꺼내쓸 수 있다.

```java
class Apple {}

class AppleBox {
	private Apple apple;

	public void putItem(Apple apple) {
		this.apple = apple;
	}

	public Apple getItem() {
		return this.apple;
	}
}
```

이번에는 망고를 보관하려고 한다면 어떨까?

```java
class Mango {}

class MangoBox {
	private Mango mango;

	public void putItem(Mango mango) {
		this.mango = mango;
	}

	public Mango getItem() {
		return this.mango;
	}
}
```

박스라는 클래스는 각각의 과일 클래스에 의존적이며, 특정 과일만을 보관하기 위해 동일한 로직을 반복적으로 작성하게 된다.

이러한 단점을 제거하기 위해 Box라는 클래스를 추상화해서 일반적으로 사용할 수 있는 클래스로 바꾸어보았다.

```java
class Box {
	private Object item;

	public void putItem(Object item) {
		this.item = item;
	}

	public Object getItem() {
		return this.item;
	}
}
```

모든 클래스는 Object 클래스를 상속받기 때문에 박스에는 사과와 망고, 그리고 그 외 모든 과일들을 담을 수 있다.
새로운 과일을 보관하려고 새로운 클래스를 만들 필요 없이 Box 클래스를 재활용할 수 있는 것이다.

```java
Box appleBox = new Box();
appleBox.putItem(new Apple());

Box mangoBox = new Box();
mangoBox.putItem(new Mango());
```

그런데 여기에는 본질적인 문제가 있다.

```java
appleBox.putItem(new Mango());

Apple apple1 = (Apple) appleBox.getItem();

// apple1 = Mango
```

Object 클래스는 모든 클래스를 담을 수 있다는 편리함이 있지만 이러한 유연함은 오류를 발생시키는 원인이 되기도 한다.
위와 같이 appleBox에 Mango를 담아도 프로그램 실행 도중에 형변환을 시도하기 때문에 코드 작성시에는 문제를 발견하기가 쉽지 않다.

이러한 점을 해결하기 위한 것이 제네릭이다. 

```java
class Box <T> {
	private T item;

	public void putItem(T item) {
		this.item = item;
	}

	public T getItem() {
		return this.item;
	}
}
```

formal type (\<T>)을 지정함으로써 해당 타입과 관련된 것들로 모두 강제적으로 정해지는 제약성을 준다.
즉 T가 Mango로 정해지면 모든 T들이 Mango가 되는 것이다.

```java
Box<Mango> mangoBox = new Box();
mangoBox.putItem(new Mango());

mangoBox.putItem(new Apple()); (X)
Mango mango1 = (Mango) mangoBox.getItem(); (X)
```

Mango로 실 타입 매개변수가 정해진 망고박스에는 이제 망고만이 담길 수 있고, 망고 박스에는 망고만 있다는 것이 보장되므로 더이상 명시적 형변환을 하지 않아도 된다!

```java
Box<Apple> appleBox = new Box<>();
appleBox.putItem(new Apple());

Box<Mango> mangoBox = new Box<>();
mangoBox.putItem(new Mango());

Box<Banana> bananaBox = new Box<>();
bananaBox.putItem(new Banana());
```

요약하면, 
1. 컴파일 시점에 타입 검사를 통해 오류를 방지하고
2. 불필요한 명시적 형변환을 제거하여 성능을 향상시킬 수 있다.

## 제네릭 사용 주의사항

#### 제네릭 타입의 객체는 생성 불가

```java
Integer[] number = new Integer[3]; // O

T Tnumber = new T();  // X
```

#### static 멤버에 제네릭 타입이 올 수 없다.

static을 정의하면 클래스가 로드 되었을 때 static memory에 객체가 올라가야 하는데 제네릭 타입의 타입 파라미터가 결정되는 것은 런타임에 인스턴스화 되었을 때이기 때문에 논리상 적용이 될 수 없다.

#### 제네릭으로 배열 선언시

기본적으로 제네릭 타입을 정의할 때 타입 파라미터를 이용한 배열 생성은 불가능하다.

```java
Box<Integer>[] boxes = new Box<>[5]; // X
```

제네릭은 컴파일시 컴파일러에 의해 타입 변환이 되며 런타임시 모두 제거된다. 따라서 Box\<Integer\>[] boxes = Box[] boxes가 될 것이다.
그러면 런타임동안 boxes에는 Integer 뿐만 아니라 String과 같은 다른 타입도 담길 수 있기 때문에 의도와 다르게 동작할 수 있는 여지가 있다.

```java
Box<?>[] boxes = new Box<?>[5];
```

와일드카드 타입을 이용한다면 모든 타입을 포함한다는 가정으로 선언이 가능하다.

해당 내용은 공변, 불공변과 와일드카드에 대한 내용을 포함하고 있는데 다음 포스팅에서 좀 더 자세하게 다루고자 한다!

## 제네릭 클래스

클래스 선언문에 제네릭 타입 매개변수를 사용한 클래스

```java
class Box<T> {
	private T name;

	public T getName() {
		return name;
	}

	public void setName(T name) {
		this.name = name;
	}
}
```

```java
Box<String> appleBox = new Box<>();
appleBox.setName("사과상자")
```


## 제네릭 인터페이스

제네릭 인터페이스를 implements 한 클래스는 오버라이딩한 메소드를 제네릭 타입에 맞춰서 구현해야 한다.

```java
interface ISample<T> {
	public void addElement(T t, int index);
	public T getElement(int index);
}

class Sample<T> implements ISameple<T> {
	private T[] array;

	public Sample() {
		array = (T[]) new object[10];
	}

	@Override
	public void addElement(T element, int index) {
		array[index] = element;
	}

	@Override
	public T getElement(int index) {
		return array[index];
	}
}
```

```java
Sample<String> sample = new Sample<>();
sample.addElement("샘플1", 1);
sample.getElement(1);
```


## 제네릭 함수형 인터페이스

```java
interface IAdd<T> {
	public T add(T x, T y);
}

public class Main {
	public static void main(String[] args) {
		IAdd<Integer> o = (x, y) -> x + y;
		
		int result = o.add(10, 20);
		System.out.println(result);  // 30
	}
}
```


## 제네릭 메소드

제네릭 메소드란, 메소드의 선언부에 \<T\>가 선언된 메소드를 의미한다.
제네릭 클래스에서 제네릭 타입 파라미터를 사용하는 메소드와는 차이가 있다.

```java
class Box<T> {
	private T item;

	// 타입 파라미터로 타입을 지정한 메소드(제네릭 메소드 X)
	public void putItem(T item) {
		this.item = item;
	}

	// 메소드 선언부에 제네릭을 선언한 제네릭 메소드
	public static <T> T addItem(T item) {
		// ...
	}
}
```

제네릭 메소드는 직접 메소드에 \<T\> 제네릭을 설정함으로써 동적으로 타입을 받아와서 사용할 수 있는, **독립적으로 운용 가능한 메소드**이다.

따라서 제네릭 클래스에 정의된 타입 매개변수와는 별개이며 제네릭 메소드의 타입 선언 위치는 **메소드 반환 타입 바로 앞**이다.

```java
// <T> : 제네릭 메소드의 제네릭 타입
public static <T> T addItem(T item) {
	// ...
}
```

### 제네릭 메소드 호출

제네릭 타입을 메소드명 왼쪽에 지정해주었듯이 호출 또한 메서드 왼쪽에 제네릭 타입이 위치한다.

```java
Box.<String>addItem("망고");

Box.addItem("망고"); // 타입 추론에 의해 생략 가능
```

제네릭 메소드가 독립적으로 운용된다는 것은 무슨 의미일까?

```java
class Box<T> {};

public static void main(String[] args) {
	// 인스턴스화에 지정된 타입 파라미터 String
	Box<String> appleBox = new Box<>();
	appleBox.putItem("망고");

	// 제네릭 메소드에 다른 타입 파라미터를 지정하면 독립적으로 운용이 된다.
	appleBox.<Integer>putItem(1);

}
```


## 제네릭 타입 범위 한정

### 클래스 타입 한정

기본 문법은 `<T extends [제한 타입]>` 이다. 이 때는 제한 타입과 제한 타입의 하위 타입들만 파라미터로 받을 수 있도록 제한된다.

```java
class Calculator<T extends Number> {
	// ...
}

// 제한 타입과 그 하위 타입들은 가능
Calculator<Number> cal1 = new Calculator<>();
Calculator<Integer> cal2 = new Calculator<>();
Calculator<Double> cal3 = new Calculator<>();

// 심지어 Object도 불가능하다.
Calculator<Object> cal4 = new Calculator<>(); // X
Calculator<String> cal5 = new Calculator<>(); // X
```

### 인터페이스 타입 한정

```java
interface Box {};

public class appleBox implements Box {};

// 인터페이스 Box를 구현한 클래스만 제네릭 가능
public class Store <T extends Box> {};
```

```java
Store<appleBox> box1 = new Store<>();
```

### 재귀적 타입 한정

재귀적 타입 한정이란 타입 매개변수가 자신을 포함하는 수식에 의해 한정되는 것을 의미한다.
주로 **Comparable 인터페이스** 와 함께 쓰인다고 한다.

```java
class Compare {
	public static <E extends Comparable<E>> E max(Collection<E> collection) {
		E result = null;
		for (E e : collection) {
			if (result == null) {
				result = e;
				continue;
			}

			if (e.compareTo(result) > 0) {
				result = e;
			}
		}
		return result;
	}
}
```

```java
Collection<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
System.out.println(Compare.max(list)); // 5
```

위 클래스는 Comparable 인터페이스를 구현해서 max값을 구하고 있다. 따라서 모든 요소들이 비교 가능해야 한다.
이 요소들이 비교 가능함을 보장하기 위해 자신과 비교가 가능한 타입만을 허용하는 데에 재귀적 타입 한정을 이용한다.

---

### 참고 자료

- [\[10분 테코톡\] 두강의 Generics](https://www.youtube.com/watch?v=n28M8iryFPw&t=382s)
- [\[코드라떼\] 자바-제네릭, 제네릭 사용하는 이유](https://www.youtube.com/watch?v=pLaVaUknETU)
- [Java Generic 제네릭 기본적인 개념 이해하기](https://wildeveloperetrain.tistory.com/103)
- [자바 제네릭스(2) Java Generics: 제네릭스는 왜 사용하는가?](https://durtchrt.github.io/blog/java/generics/2/)
- [제네릭 배열을 생성하지 못하는 이유](https://pompitzz.github.io/blog/Java/whyCantCreateGenericsArray.html#%E1%84%8C%E1%85%A6%E1%84%82%E1%85%A6%E1%84%85%E1%85%B5%E1%86%A8%E1%84%80%E1%85%AA-%E1%84%87%E1%85%A2%E1%84%8B%E1%85%A7%E1%86%AF%E1%84%8B%E1%85%B4-%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5%E1%84%8C%E1%85%A5%E1%86%B7)
- [제네릭 배열을 사용하지 말아야 하는 이유](https://wraithkim.wordpress.com/2015/09/09/java-%EC%A0%9C%EB%84%88%EB%A6%AD-%EB%B0%B0%EC%97%B4%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%EB%90%98%EB%8A%94-%EC%9D%B4%EC%9C%A0/)
- [자바 제네릭(Generics) 개념 & 문법 정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%9C%EB%84%A4%EB%A6%ADGenerics-%EA%B0%9C%EB%85%90-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)