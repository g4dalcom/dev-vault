# [문제링크](https://www.acmicpc.net/problem/5430)

## 📝 문제

선영이는 주말에 할 일이 없어서 새로운 언어 AC를 만들었다. AC는 정수 배열에 연산을 하기 위해 만든 언어이다. 이 언어에는 두 가지 함수 R(뒤집기)과 D(버리기)가 있다.

함수 R은 배열에 있는 수의 순서를 뒤집는 함수이고, D는 첫 번째 수를 버리는 함수이다. 배열이 비어있는데 D를 사용한 경우에는 에러가 발생한다.

함수는 조합해서 한 번에 사용할 수 있다. 예를 들어, "AB"는 A를 수행한 다음에 바로 이어서 B를 수행하는 함수이다. 예를 들어, "RDD"는 배열을 뒤집은 다음 처음 두 수를 버리는 함수이다.

배열의 초기값과 수행할 함수가 주어졌을 때, 최종 결과를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다. T는 최대 100이다.

각 테스트 케이스의 첫째 줄에는 수행할 함수 p가 주어진다. p의 길이는 1보다 크거나 같고, 100,000보다 작거나 같다.

다음 줄에는 배열에 들어있는 수의 개수 n이 주어진다. (0 ≤ n ≤ 100,000)

다음 줄에는 [x1,...,xn]과 같은 형태로 배열에 들어있는 정수가 주어진다. (1 ≤ xi ≤ 100)

전체 테스트 케이스에 주어지는 p의 길이의 합과 n의 합은 70만을 넘지 않는다.

## 출력

각 테스트 케이스에 대해서, 입력으로 주어진 정수 배열에 함수를 수행한 결과를 출력한다. 만약, 에러가 발생한 경우에는 error를 출력한다.

## 예제 입력 1 

4
RDD
4
[1,2,3,4]
DD
1
[42]
RRD
6
[1,1,2,3,5,8]
D
0
[]

## 예제 출력 1

[2,1]
error
[1,2,3,5,8]
error

---

### ❌ 오답 (첫번째 시도)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());

        StringBuilder sb = new StringBuilder();
        while (T-- > 0) {
            String input = br.readLine();
            char[] inputToChar = input.toCharArray();

            int arraySize = Integer.parseInt(br.readLine());
            if (arraySize == 0) sb.append("error");

            String arrayInput = br.readLine();
            arrayInput = arrayInput.replace("[", "");
            arrayInput = arrayInput.replace("]", "");
            String[] toArray = arrayInput.split(",");

            ArrayList<String> list = new ArrayList<>(Arrays.asList(toArray));

            for (int i = 0; i < inputToChar.length; i++) {
                if (inputToChar[i] == 'R') {
                    Collections.reverse(list);
                } else if (inputToChar[i] == 'D') {
                    if (list.size() == 0) {
                        sb.append("error");
                    }
                    else list.remove(0);
                }
            }

            for (int i = 0; i < list.size(); i++) {
                if (i+1 == list.size()) sb.append(list.get(i)).append("]");
                else if (i == 0) sb.append("[").append(list.get(i)).append(",");
                else sb.append(list.get(i)).append(",");
            }
            sb.append("\n");
        }
        System.out.println(sb);
    }
}
```
- 입력값을 배열에 담는 방법도 비효율적이었고
- 직접 배열을 뒤집어주면 무조건 시간초과가 나기 때문에 Deque를 이용해서 해결해야 된다는 힌트를 얻고 Deque와 정규표현식을 공부한 뒤에 풀어보았다.

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.StringTokenizer;

public class Main {
    static StringBuilder sb = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());        // 테스트 케이스 개수

        while (T-- > 0) {
            String command = br.readLine();             // 커맨드 입력값
            int n = Integer.parseInt(br.readLine());    // 배열 크기

            StringTokenizer st = new StringTokenizer(br.readLine(), "[],");

            ArrayDeque<Integer> dq = new ArrayDeque<>();
            for (int i = 0; i < n; i++) {
                dq.add(Integer.parseInt(st.nextToken()));
            }

            AC(command, dq);
        }
        System.out.println(sb);
    }
    public static void AC(String command, ArrayDeque<Integer> dq) {
        boolean isFront = true;         // 배열을 직접 뒤집지 않고 deque에서 앞뒤를 구분할 변수

        for (char cmd : command.toCharArray()) {

            if (cmd == 'R') {
                isFront = !isFront;     // 커맨드가 R이라면 isFront 함수를 반대로 바꾸어준다.
                continue;
            }

            
            // 커맨드가 'D' 인 경우,
            // 정방향이라면 앞에서부터 poll() 하고, null이 발생하면 error를 출력한다.
            if (isFront) {
                if (dq.pollFirst() == null) {
                    sb.append("error").append("\n");
                    return;
                }

            // 역방향이라면 뒤에서부터 poll()
            } else {
                if (dq.pollLast() == null) {
                    sb.append("error").append("\n");
                    return;
                }
            }
        }

        makePrintString(dq, isFront);
    }

    public static void makePrintString(ArrayDeque<Integer> dq, boolean isFront) {

        // 괄호 열어주기
        sb.append("[");

        // 빈 배열이 반환될 경우에는 아래 조건문을 건너뛰고 "[]" 만 출력하게 된다.
        if (dq.size() > 0) {

            // 정방향일 경우
            if (isFront) {
                sb.append(dq.pollFirst());                      // 먼저 첫 번째 요소 넘기기

                while (!dq.isEmpty()) {
                    sb.append(",").append(dq.pollFirst());      // 그 다음부터 "," + 요소를 넘겨준다.
                }

            // 역방향일 경우
            } else {
                sb.append(dq.pollLast());                       // 뒤에서 첫 번째 요소 넘기기

                while (!dq.isEmpty()) {
                    sb.append(",").append(dq.pollLast());       // 그 다음부터 "," + 요소를 넘겨준다.
                }
            }
        }
        sb.append("]").append("\n");                            // 괄호 닫고 개행!
    }
}
```
- [풀이 참고사이트](https://st-lab.tistory.com/221)


### 문자열 나누기

```java
StringTokenizer st = new StringTokenizer(br.readLine(), "[],");
```
	
- 문자열을 여러 조건으로 나눌 때, StringTokenizer는 정규식 검사가 아니기 때문에 구분하려는 문자들을 하나의 문자열로 인자를 넘겨주면 된다.
- 하지만 권장 방식은 split() 라고 한다.

```java
str = "[1,2,3,4]";
strr[] = str.split("[\\[\\]\\,]");

/**  
 *  result)
 *  strr[0] = ""
 *  strr[1] = "1"
 *  strr[2] = "2"
 *  strr[3] = "3"
 *  strr[4] = "4"
 */
```

- 위와 같이정규표현식을 사용해서 split하게 되면 첫 번째 인자가 정규식에 걸려서 빈 문자열을 반환하기 때문에 

```java
String input = br.readLine();
String[] s = input.subString(1, input.length - 1).split(",");
```

- 처음과 끝에 있는 대괄호를 잘라내고 split(",") 해주어야 한다.