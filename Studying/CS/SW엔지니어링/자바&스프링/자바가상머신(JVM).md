---
keyword : Java
class : CS
---


### JVM

### JVM(Java Virtual Machine)이란?

자바 가상 머신으로 자바 바이트 코드를 실행할 수 있는 주체 즉 Java Byte Code를 OS에 맞게 해석 해주는 역할을 한다.

CPU나 운영체제(플랫폼)의 종류와 무관하게 실행이 가능하다. 즉 OS에 종속적이지 않고 Java 파일 하나만 만들면 어느 디바이스든 JVM 위에서 실행 할 수 있다.

즉, 운영체제 위에서 동작하는 프로세스로 자바 코드를 컴파일해서 얻은 바이트 코드를 해당 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 한다.

Java compiler는 .java 파일을 .class 라는 Java byte code로 변환시킨다. Byte Code 는 기계어가 아니기 때문에OS에서 바로 실행되지 않는데 이때 JVM은 OS가 ByteCode를 이해할 수 있도록 해석한다.


### 1. Class Loader

JAVA 컴파일러에 의해 .JAVA 파일이 .class로 컴파일 되는데 이 때 만들어진 클래스 파일을 RunTime 시점에 로딩하게 해주며 클래스의 인스턴스를 생성하면 클래스 로더를 통해 메모리에 로드하게 됩니다.

### 2. Execution Engine

Load된 Class의 ByteCode를 실행하는 Runtime Module이 바로 Execution Engine입니다. Class Loader를 통해 JVM 내의 Runtime Data Areas 에 배치된 바이트 코드는 Executin Engine에 의해 실행되며, 실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행합니다.

최초 JVM 이 나왔을 당시에는 Interperter방식(한 줄씩 해석하고 실행)이였기 때문에 속도가 느리다는 단점이 있었지만 JIT complier 방식을 통해 이 점을 보완했습니다. JIT는 ByteCode를 어셈블러 같은 NativeCode로 바꿔서 실행이 빠르지만 역시 변환하는데 비용이 발생합니다. 이 같은 이유 때문에 JVM은 모든 코드를 JIT Compiler 방식으로 실행하지 않고 Interpreter 방식을 사용하다 일정한 기준이 넘어가면 JIT Compiler 방식으로 실행합니다.

### 3. Garbage Collector

Garbage Collector(GC)는 Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다. GC가 역할을 하는 시간은 정확히 언제인지를 알 수 없다.

### 4. Runtime Data Area

JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다. 이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있다.


	### 4-1. Method area (메소드 영역)

		클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보와 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보, Type정보(Interface인지 class인지), Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨), static 변수, final class 변수등이 생성되는 영역이다.

	### 4-2. Heap area (힙 영역)

		new 키워드로 생성된 객체와 배열이 생성되는 영역이다. 메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역이다.

	### 4-3. Stack area (스택 영역)

		지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값등이 생성되는 영역이다.
		
		int a = 10; 이라는 소스를 작성했다면 정수값이 할당될 수 있는 메모리공간을 a라고 잡아두고 그 메모리 영역에 값이 10이 들어간다. 즉, 스택에 메모리에 이름이 a라고 붙여주고 값이 10인 메모리 공간을 만든다.
		
		클래스 Person p = new Person(); 이라는 소스를 작성했다면 Person p는 스택 영역에 생성되고 new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.
		
		그리고 스택영역에 생성된 p의 값으로 힙 영역의 주소값을 가지고 있다. 즉, 스택 영역에 생성된 p가 힙 영역에 생성된 객체를 가리키고(참조하고) 있는 것이다.
		
		메소드를 호출할 때마다 개별적으로 스택이 생성된다.

	### 4-4. PC Register (PC 레지스터)

		Thread(쓰레드)가 생성될 때마다 생성되는 영역으로 Program Counter 즉, 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다. (*CPU의 레지스터와 다름) 이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.

	### 4-5. Native method stack

		자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다. 보통 C/C++등의 코드를 수행하기 위한 스택이다. (JNI)

---

[깃허브-1](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EA%B0%80%EC%83%81%20%EB%A8%B8%EC%8B%A0(Java%20Virtual%20Machine).md)

[깃허브-2](https://github.com/jobhope/TechnicalNote/blob/master/java/JVM.md)

[JVM이란 무엇인가](https://velog.io/@tpdns90321/Spring-Study-1.-JVM%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

[JVM이란?](https://devlogofchris.tistory.com/65)