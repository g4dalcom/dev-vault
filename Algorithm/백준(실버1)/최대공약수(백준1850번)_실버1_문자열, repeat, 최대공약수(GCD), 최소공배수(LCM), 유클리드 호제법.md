# [문제링크](https://www.acmicpc.net/problem/1850)

## 📝 문제

모든 자리가 1로만 이루어져있는 두 자연수 A와 B가 주어진다. 이때, A와 B의 최대 공약수를 구하는 프로그램을 작성하시오.

예를 들어, A가 111이고, B가 1111인 경우에 A와 B의 최대공약수는 1이고, A가 111이고, B가 111111인 경우에는 최대공약수가 111이다.

## 입력

첫째 줄에 두 자연수 A와 B를 이루는 1의 개수가 주어진다. 입력되는 수는 263보다 작은 자연수이다.

## 출력

첫째 줄에 A와 B의 최대공약수를 출력한다. 정답은 천만 자리를 넘지 않는다.

## 예제 입력 1 

3 4

## 예제 출력 1 

1

## 예제 입력 2 
3 6

## 예제 출력 2 

111

## 예제 입력 3 

500000000000000000 500000000000000002

## 예제 출력 3

11

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        long a = Long.parseLong(st.nextToken());
        long b = Long.parseLong(st.nextToken());

        long ea = GCD(a, b);

        String tmp = "1".repeat((int) ea);
        
        System.out.println(tmp);

    }
    public static long GCD(long x, long y) {
        if (y == 0) return x;

        return GCD(y, x % y);
    }
}
```
- [참고 사이트](https://soojong.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%EC%9E%90%EB%B0%94-%EB%B0%B1%EC%A4%80-1850%EB%B2%88-%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98)
- 전에 최대공약수(GCD)와 최소공배수(LCM)를 구하는 공식을 봤던 적이 있어서 적용해보았는데 시간초과가 떴다.
- 고민을 하다가 참고 사이트를 보고나서야 문제에 규칙이 있다는 것을 알 수 있었다.
- 주어진 두 자연수의 최대공약수가 1의 개수가 되는 것인데 이 규칙을 어떻게 찾았을까 궁금하다 ㅠㅠ 다른 풀이들을 보면 다들 쉽게 찾았다고 하는데... 수학에 약해서 못찾은 것인지 ㅠㅠ
- 어쨌든 이번 문제를 풀면서 최대공약수, 최소공배수를 알고리즘으로 해결하는 공식을 다시 리마인드할 수 있었다.

### 최대공약수(재귀)

```java
public static int GCD(int a, int b) {
	if (b == 0) return a;

	return GCD(b, a % b);
}
```

### 최대공약수(반복문)

```java
public static int GCD(int a, int b) {
	while (b > 0) {
		int tmp = a;
		a = b;
		b = tmp % a;
	}
	return a;
}
```

### 최소공배수

```java
LCM = (a * b) / GCD(a, b);
```

- 그리고 또 한 가지는, repeat 메소드이다.
- 최대공약수를 구하고 그 수만큼 1을 붙이기 위해 String 변수를 선언하고 for문을 돌면서 변수에 1을 붙여주었는데 이 때도 시간초과가 발생하였다.
- 결국 StringBuilder로 해결하긴 했지만 repeat메소드 또한 문자열을 반복할 때 좋은 성능을 내는 API인 것 같다!