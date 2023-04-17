---
keyword : Python
class : Grammer, Algorithm
---


### input()

[python documentation - input()](https://docs.python.org/3/library/functions.html#input)  
공식 문서의 정의에 따르면(직역 주의)

> If the prompt argument is present, it is written to standard output without a trailing newline.  
> => 인자가 존재한다면, 추가 개행 없이 표준 출력에 기록된다  
>   
>   
> The function then reads a line from input, converts it to a string (stripping a trailing newline), and returns that.  
> => 입력으로부터 한 줄을 읽어들인 뒤, 문자열로 변환 후(개행은 벗겨내고) 반환한다.

즉, `input()`은 `사용자의 입력을 받고` → `문자열로 변환` → `추가 strip 진행` 의 과정을 거치는 것이다.  
  
또한 `input()`은 사용자로부터 입력을 받기 전 이를 기다리기 위해 prompt를 가지고 있다. 때문에 대량의 입력을 받는 경우라면 `입력을 받고 prompt를 띄우고` 의 과정을 반복하므로 오류가 발생할 가능성이 존재한다. (여러 자료를 참고해 제 방식대로 설명한 내용이라 정확하지 않을 수 있습니다.)


---


### sys.stdin.readline()

`stdin` 은 `standard input`을 뜻하며 얼핏 보면 `input()` 과 같은 동작을 수행한다고 생각할 수 있다.

`sys.stdin.readline()`은 사용자의 입력을 받지만 개행 문자도 입력을 받을 수 있다. 또한 입력 크기에 제한을 줌으로써 한번에 읽어들일 문자의 수를 정할 수 있다.

```python
num = sys.stdin.readline(2) # 입력 : 1234
print(num) # 결과 : 12
```

[python documentation - sys](https://docs.python.org/3/library/sys.html)  
`input()`과 가장 큰 차이점은 `input()` 은 **내장 함수**로 취급되는 반면 `sys` 에 속하는 메소드들은 **file object**로 취급된다. 즉, 사용자의 입력만을 받는 buffer를 하나 만들어 그 buffer에서 읽어들이는 것이다.

`input()`은 더 이상 입력이 없는데도 수행될 경우 EOFerror를 뱉어내는 반면 `sys.stdin.readline()`은 빈 문자열을 반환한다. 어떻게 보면 에러에서도 안전할 듯?

---

**참고 자료**  
[stackoverflow : sys.stdin.readline() and input(): which one is faster when reading lines of input, and why?](https://stackoverflow.com/questions/22623528/sys-stdin-readline-and-input-which-one-is-faster-when-reading-lines-of-inpu)

[GeeksforGeeks : Difference between input() and sys.stdin.readline()](https://www.geeksforgeeks.org/difference-between-input-and-sys-stdin-readline/)

[Quora : What is the difference between input() and sys.stdin in Python?](https://www.quora.com/What-is-the-difference-between-input-and-sys-stdin-in-Python)
