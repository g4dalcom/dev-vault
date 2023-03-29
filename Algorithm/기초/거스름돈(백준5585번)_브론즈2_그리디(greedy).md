# [문제링크](https://www.acmicpc.net/problem/5585)

## 📝 문제

타로는 자주 JOI잡화점에서 물건을 산다. JOI잡화점에는 잔돈으로 500엔, 100엔, 50엔, 10엔, 5엔, 1엔이 충분히 있고, 언제나 거스름돈 개수가 가장 적게 잔돈을 준다. 타로가 JOI잡화점에서 물건을 사고 카운터에서 1000엔 지폐를 한장 냈을 때, 받을 잔돈에 포함된 잔돈의 개수를 구하는 프로그램을 작성하시오.

## 입력

입력은 한줄로 이루어져있고, 타로가 지불할 돈(1 이상 1000미만의 정수) 1개가 쓰여져있다.

## 출력

제출할 출력 파일은 1행으로만 되어 있다. 잔돈에 포함된 매수를 출력하시오.

## 예제 입력 1 

380

## 예제 출력 1 

4

## 예제 입력 2

1

## 예제 출력 2 

15

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        int cnt = 0;
        int change = 1000 - n;

        while (change > 0) {

            if (change >= 500) {
                change -= 500;
            } else if (change >= 100) {
                change -= 100;
            } else if (change >= 50) {
                change -= 50;
            } else if (change >= 10) {
                change -= 10;
            } else if (change >= 5) {
                change -= 5;
            } else {
                change -= 1;
            }
            cnt++;
        }

        System.out.println(cnt);
    }
}
```


### 🔎 다른 풀이

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        int cnt = 0;
        int change = 1000 - n;
        int[] coin = {500, 100, 50, 10, 5, 1};

        for (int i = 0; i < coin.length; i++) {
            if (change / coin[i] > 0) {
                cnt += change / coin[i];
                change = change % coin[i];
            }
        }

        System.out.println(cnt);
    }
}
```
- 뺄셈보다 나눗셈의 몫을 이용하면 한 번의 나눗셈으로 해당 동전으로 할 수 있는 카운팅을 모두 할 수 있다!