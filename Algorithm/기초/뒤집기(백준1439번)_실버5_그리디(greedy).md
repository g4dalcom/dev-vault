# [문제링크](https://www.acmicpc.net/problem/1439)

## 📝 문제

다솜이는 0과 1로만 이루어진 문자열 S를 가지고 있다. 다솜이는 이 문자열 S에 있는 모든 숫자를 전부 같게 만들려고 한다. 다솜이가 할 수 있는 행동은 S에서 연속된 하나 이상의 숫자를 잡고 모두 뒤집는 것이다. 뒤집는 것은 1을 0으로, 0을 1로 바꾸는 것을 의미한다.

예를 들어 S=0001100 일 때,

1.  전체를 뒤집으면 1110011이 된다.
2.  4번째 문자부터 5번째 문자까지 뒤집으면 1111111이 되어서 2번 만에 모두 같은 숫자로 만들 수 있다.

하지만, 처음부터 4번째 문자부터 5번째 문자까지 문자를 뒤집으면 한 번에 0000000이 되어서 1번 만에 모두 같은 숫자로 만들 수 있다.

문자열 S가 주어졌을 때, 다솜이가 해야하는 행동의 최소 횟수를 출력하시오.

## 입력

첫째 줄에 문자열 S가 주어진다. S의 길이는 100만보다 작다.

## 출력

첫째 줄에 다솜이가 해야하는 행동의 최소 횟수를 출력한다.

## 예제 입력 1 

0001100

## 예제 출력 1 

1

## 예제 입력 2 

11111

## 예제 출력 2 

0

## 예제 입력 3 

00000001

## 예제 출력 3 

1

## 예제 입력 4 

11001100110011000001

## 예제 출력 4 

4

## 예제 입력 5 

11101101

## 예제 출력 5 

2

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] S = br.readLine().split("");

        boolean flag = false;
        int ZeroCount = 0;
        int OneCount = 0;
        
        flag = S[0].equals("0");
        for (int i = 0; i < S.length; i++) {
            if (S[i].equals("1") && !flag) {
                OneCount++;
                flag = true;
            }
            else if (S[i].equals("0") && flag) {
                ZeroCount++;
                flag = false;
            }
        }
        System.out.println(Math.min(ZeroCount, OneCount));
    }
}
```
- 연속된 같은 문자열을 덩어리로 묶어서 풀어보았다. 예를 들어서 0001100 이라면, 0은 2묶음, 1은 1묶음으로 최솟값인 1묶음을 리턴하게 된다.
- 문자가 "0" 이면서 flag = false 일 때는 0인 문자가 계속되는 상태이고
- 문자가 "1" 이면서 flag = true 일 때는 1인 문자가 계속되는 상태이다.
- flag의 상태가 반대일 경우는 이전에 다른 문자가 나왔었다는 뜻이므로 묶음의 개수를 + 해주는 방식이다!
- boolean 의 기본값이 false인데 처음에 "0"이 나오는 경우에 묶음의 개수를 세어주기 위해 flag의 초깃값을 아래와 같이 조건으로 넣어주었다!

```java
flag = S[0].equals("0");
```