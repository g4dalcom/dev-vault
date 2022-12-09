# [문제링크](https://www.acmicpc.net/problem/1541)

## 📝 문제

세준이는 양수와 +, -, 그리고 괄호를 가지고 식을 만들었다. 그리고 나서 세준이는 괄호를 모두 지웠다.

그리고 나서 세준이는 괄호를 적절히 쳐서 이 식의 값을 최소로 만들려고 한다.

괄호를 적절히 쳐서 이 식의 값을 최소로 만드는 프로그램을 작성하시오.

## 입력

첫째 줄에 식이 주어진다. 식은 ‘0’~‘9’, ‘+’, 그리고 ‘-’만으로 이루어져 있고, 가장 처음과 마지막 문자는 숫자이다. 그리고 연속해서 두 개 이상의 연산자가 나타나지 않고, 5자리보다 많이 연속되는 숫자는 없다. 수는 0으로 시작할 수 있다. 입력으로 주어지는 식의 길이는 50보다 작거나 같다.

## 출력

첫째 줄에 정답을 출력한다.

## 예제 입력 1 

55-50+40

## 예제 출력 1 

-35

## 예제 입력 2 

10+20+30+40

## 예제 출력 2 

100

## 예제 입력 3 

00009-00009

## 예제 출력 3

0


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.*;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        StringTokenizer st = new StringTokenizer(br.readLine(), "-");  
  
        ArrayList<Integer> arr = new ArrayList<>();  
        while (st.hasMoreTokens()) {  
            String s = st.nextToken();  
            StringTokenizer st2 = new StringTokenizer(s, "+");  
  
            int sum = 0;  
            while (st2.hasMoreTokens()) {  
                sum += Integer.parseInt(st2.nextToken());  
            }  
            arr.add(sum);  
        }  
  
        int result = arr.get(0);  
        for (int i = 1; i < arr.size(); i++) {  
            result -= arr.get(i);  
        }  
  
        System.out.println(result);  
    }  
}
```
- 가장 작은 수를 만들기 위해서는 가장 큰 수를 빼주어야 한다.
- "-" 사이에 있는 값들을 모두 더하면 최소값이 나올 수 있다.
	- 55 - 50 + 40 + 20 - 10 + 20 + 30 이라면,
	- 55 - (50 + 40 + 20) - (10 + 20 + 30) 이런 식이 된다!
- 1차적으로 "-" 를 기준으로 문자열을 구분하고!
	- 55, 50+40+20, 10+20+30
- 그 다음에 "+" 기준으로 나누면서 값들을 더해준다.
	- arr.add(55)
	- arr.add(50+40+20)
	- arr.add(10+20+30)
- 처음과 마지막은 숫자만 온다고 했으므로 처음 수는 무조건 양수일 수 밖에 없기 때문에 결과로 내보낼 변수(result)에 arr.get(0)을 먼저 넣어놓고 나머지 인덱스들은 모두 빼주면 된다!
