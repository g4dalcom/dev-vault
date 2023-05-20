# 제네릭이란?!

- 제네릭은 컴파일 단계에서 자료형을 체크함으로써 코드의 안정성을 높여주는 도구
- 데이터를 저장하는 시점에 어떤 데이터를 저장할 것인지 명시할 수 있음


## 제네릭의 필요성

- 사과를 박스에 담아서 보관하려고 한다고 가정해보자!
- 아래와 같이 사과 클래스를 가져와서 사과 상자에 담고 필요할 때 꺼내쓸 수 있다.

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

- 이번에는 망고를 보관하려고 한다면 어떨까?

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

- 박스라는 클래스는 각각의 과일 클래스에 의존적이며, 특정 과일만을 보관하기 위해 동일한 로직을 반복적으로 작성하게 된다.
- 이러한 단점을 제거하기 위해 Box라는 클래스를 추상화해서 일반적으로 사용할 수 있는 클래스로 바꾸어보았다.

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

- 모든 클래스는 Object 클래스를 상속받기 때문에 박스에는 사과와 망고, 그리고 그 외 모든 과일들을 담을 수 있다.
- 새로운 과일을 보관하려고 새로운 클래스를 만들 필요 없이 Box 클래스를 재활용할 수 있는 것이다.

```java
Box appleBox = new Box();
appleBox.putItem(new Apple());

Box mangoBox = new Box();
mangoBox.putItem(new Mango());
```

- 그런데 여기에는 본질적인 문제가 있다.

```java
appleBox.putItem(new Mango());

Apple apple1 = (Apple) appleBox.getItem();

// apple1 = Mango
```

- Object 클래스는 모든 클래스를 담을 수 있다는 편리함이 있지만 이러한 유연함은 오류를 발생시키는 원인이 되기도 한다.
- 위와 같이 appleBox에 Mango를 담아도 프로그램 실행 도중에 형변환을 시도하기 때문에 코드 작성시에는 문제를 발견하기가 쉽지 않다.
- 이러한 점을 해결하기 위한 것이 제네릭이다. 

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

- formal type (\<T>)을 지정함으로써 해당 타입과 관련된 것들로 모두 강제적으로 정해지는 제약성을 준다.
- 즉 T가 Mango로 정해지면 모든 T들이 Mango가 되는 것이다.

```java
Box<Mango> mangoBox = new Box();
mangoBox.putItem(new Mango());

mangoBox.putItem(new Apple()); (X)
Mango mango1 = (Mango) mangoBox.getItem(); (X)
```

- Mango로 실 타입 매개변수가 정해진 망고박스에는 이제 망고만이 담길 수 있고, 망고 박스에는 망고만 있다는 것이 보장되므로 더이상 명시적 형변환을 하지 않아도 된다!

```java
Box<Apple> appleBox = new Box<>();
appleBox.putItem(new Apple());

Box<Mango> mangoBox = new Box<>();
mangoBox.putItem(new Mango());

Box<Banana> bananaBox = new Box<>();
bananaBox.putItem(new Banana());
```

- 제네릭으로 설계된 추상화된 Box라는 클래스를 이용하여 원하는 안정된 결과값을 얻을 수 있다!


### 타입 변수

> formal type parameter(형식 타입 매개변수)는 통상적으로 **E: Element, K, V: Key, Value, N: Number, T: 일반적인 제네릭 타입 변수** 로 사용되나 상황에 맞게 의미있는 문자를 선택해서 사용하면 된다고 한다!

- 제네릭이 선언된 클래스는 actual type parameter(실 타입 매개변수)를 통해 **참조 자료형**을 선언할 수 있다.

> 참조 자료형만 선언할 수 있기 때문에 int, double, char 같은 원시 타입은 Integer, Double, Character와 같은 Wrapper type으로 변환하여 사용해야 한다.



---

### 참고 자료

- [\[10분 테코톡\] 두강의 Generics](https://www.youtube.com/watch?v=n28M8iryFPw&t=382s)
- [\[코드라떼\] 자바-제네릭, 제네릭 사용하는 이유](https://www.youtube.com/watch?v=pLaVaUknETU)
- [Java Generic 제네릭 기본적인 개념 이해하기](https://wildeveloperetrain.tistory.com/103)
- [자바 제네릭스(2) Java Generics: 제네릭스는 왜 사용하는가?](https://durtchrt.github.io/blog/java/generics/2/)