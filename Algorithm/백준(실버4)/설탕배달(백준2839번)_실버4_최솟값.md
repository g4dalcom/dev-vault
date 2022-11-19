# [문제링크](https://www.acmicpc.net/problem/2839)

## 📝 문제

상근이는 요즘 설탕공장에서 설탕을 배달하고 있다. 상근이는 지금 사탕가게에 설탕을 정확하게 N킬로그램을 배달해야 한다. 설탕공장에서 만드는 설탕은 봉지에 담겨져 있다. 봉지는 3킬로그램 봉지와 5킬로그램 봉지가 있다.

상근이는 귀찮기 때문에, 최대한 적은 봉지를 들고 가려고 한다. 예를 들어, 18킬로그램 설탕을 배달해야 할 때, 3킬로그램 봉지 6개를 가져가도 되지만, 5킬로그램 3개와 3킬로그램 1개를 배달하면, 더 적은 개수의 봉지를 배달할 수 있다.

상근이가 설탕을 정확하게 N킬로그램 배달해야 할 때, 봉지 몇 개를 가져가면 되는지 그 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (3 ≤ N ≤ 5000)

## 출력

상근이가 배달하는 봉지의 최소 개수를 출력한다. 만약, 정확하게 N킬로그램을 만들 수 없다면 -1을 출력한다.

## 예제 입력 1

18

## 예제 출력 1 

4

## 예제 입력 2

4

## 예제 출력 2 

-1

## 예제 입력 3 

6

## 예제 출력 3 

2

## 예제 입력 4 

9

## 예제 출력 4 

3

## 예제 입력 5 

11

## 예제 출력 5 

3


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        int N = Integer.parseInt(br.readLine());  
  
        int cnt = 0;  
        while (true) {  
            if (N % 5 == 0) {  
              cnt += N / 5;  
                System.out.println(cnt);  
              break;  
            } else {  
                N = N - 3;  
                cnt++;  
            }  
            if (N < 0) {  
                System.out.println(-1);  
                break;  
            }  
        }  
    }  
}
```
- 최대한 5kg봉지를 사용하는 것이 최소한의 봉지를 가져가게 되는 것이므로 우선, N이 5의 배수라면 5로 나눈 값을 출력하면 되고
- 5의 배수가 아니라면 5의 배수가 될 때까지 3을 한 번씩 빼주면서 cnt를 늘려갔다
- 만약 N이 3을 계속 뺐는데 음수가 나온다면 3kg, 5kg 봉지로 만들 수 없는 수이므로 -1을 출력하도록 하였다.
