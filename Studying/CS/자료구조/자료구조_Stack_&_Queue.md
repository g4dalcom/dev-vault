---
keyword : Data structure
class : CS
---

# Stack and Queue

### Stack

선형 자료구조의 일종으로 `Last In First Out (LIFO)` - 나중에 들어간 원소가 먼저 나온다. 또는 `First In Last Out (FILO)` - 먼저 들어간 원소가 나중에 나온다. 이것은 Stack 의 가장 큰 특징이다. 차곡차곡 쌓이는 구조로 먼저 Stack 에 들어가게 된 원소는 맨 바닥에 깔리게 된다. 그렇기 때문에 늦게 들어간 녀석들은 그 위에 쌓이게 되고 호출 시 가장 위에 있는 녀석이 호출되는 구조이다.

### Queue

선형 자료구조의 일종으로 `First In First Out (FIFO)`. 즉, 먼저 들어간 놈이 먼저 나온다. Stack 과는 반대로 먼저 들어간 놈이 맨 앞에서 대기하고 있다가 먼저 나오게 되는 구조이다. 참고로 Java Collection 에서 Queue 는 인터페이스이다. 이를 구현하고 있는 `Priority queue`등을 사용할 수 있다.