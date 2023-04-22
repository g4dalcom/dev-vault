# [문제링크](https://www.acmicpc.net/problem/1339)

## 📝 문제

민식이는 수학학원에서 단어 수학 문제를 푸는 숙제를 받았다.

단어 수학 문제는 N개의 단어로 이루어져 있으며, 각 단어는 알파벳 대문자로만 이루어져 있다. 이때, 각 알파벳 대문자를 0부터 9까지의 숫자 중 하나로 바꿔서 N개의 수를 합하는 문제이다. 같은 알파벳은 같은 숫자로 바꿔야 하며, 두 개 이상의 알파벳이 같은 숫자로 바뀌어지면 안 된다.

예를 들어, GCF + ACDEB를 계산한다고 할 때, A = 9, B = 4, C = 8, D = 6, E = 5, F = 3, G = 7로 결정한다면, 두 수의 합은 99437이 되어서 최대가 될 것이다.

N개의 단어가 주어졌을 때, 그 수의 합을 최대로 만드는 프로그램을 작성하시오.

## 입력

첫째 줄에 단어의 개수 N(1 ≤ N ≤ 10)이 주어진다. 둘째 줄부터 N개의 줄에 단어가 한 줄에 하나씩 주어진다. 단어는 알파벳 대문자로만 이루어져있다. 모든 단어에 포함되어 있는 알파벳은 최대 10개이고, 수의 최대 길이는 8이다. 서로 다른 문자는 서로 다른 숫자를 나타낸다.

## 출력

첫째 줄에 주어진 단어의 합의 최댓값을 출력한다.

## 예제 입력 1 

2
AAA
AAA

## 예제 출력 1 

1998

## 예제 입력 2

2
GCF
ACDEB

## 예제 출력 2 

99437

## 예제 입력 3 

10
A
B
C
D
E
F
G
H
I
J

## 예제 출력 3

45

## 예제 입력 4 

2
AB
BA

## 예제 출력 4 

187

---

### ❌ 오답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        // 우선순위 큐에 String 길이를 기준으로 내림차순 정렬!
        PriorityQueue<String> pq = new PriorityQueue<>(new Comparator<String>() {
            @Override
            public int compare(final String o1, final String o2) {
                return o2.length() - o1.length();
            }
        });

        Queue<String> result = new LinkedList<>();

        for (int i = 0; i < N; i++) {
            String input = br.readLine();
            pq.offer(input);
            result.offer(input);
        }

        HashMap<String, Integer> map = new LinkedHashMap<>();

        /**
         * 우선순위 큐에서 가장 긴 문자열부터 꺼내서 0번 인덱스 문자를 해시맵에 넣어줌!
         * 가장 먼저 들어가는 문자가 9를 받고 그 이후부터는 하나씩 작아지는 수를 받는다.
         * 그리고 0번 문자를 뺀 문자를 다시 우선순위 큐에 넣기!
         */
        int numbers = 9;
        while (!pq.isEmpty()) {
            String temp = pq.poll();
            String alphabet = temp.substring(0, 1);
            if (!map.containsKey(alphabet)) {
                map.put(alphabet, numbers);
                numbers--;
            }

            if (temp.length() > 1) {
                String newTemp = temp.substring(1);
                pq.offer(newTemp);
            }
        }

        // 입력받은 문자열의 문자와 해시맵의 키를 비교해서 해당 값을 받아온 후 계산해주기
        int size = result.size();
        int sum = 0;
        for (int i = 0; i < size; i++) {
            String[] check = result.poll().split("");
            String conversion = "";

            for (int j = 0; j < check.length; j++) {
                if (map.containsKey(check[j])) {
                    conversion += map.get(check[j]);
                }
            }

            sum += Integer.parseInt(conversion);
        }
        
        System.out.println(sum);
    }
}
```
- 예제를 보면서 고민해보았는데, 가장 긴 문자열의 맨 앞 문자부터 큰 수를 부여하면 될 것이라고 생각하였다.

```java
2
GCF
ACDEB
```
- 위 예제를 예로 들면, A, C 가 각각 9, 8을 부여받고 G나 D가 그 다음 수인 7과 6, C는 중복이므로 계산하지 않는 식으로 풀어보려고 하였다.
- 우선 위 풀이대로면 예제 문제들은 통과가 된다. 그러나 제출하자마자 고민도 안 하고 틀렸다고 내뱉는 것을 보고 반례를 찾아보았는데, 나의 접근이 완전히 틀렸다는 것을 알게 되었다 ㅠㅠ

```java
10
ABB
BB
BB
BB
BB
BB
BB
BB
BB
BB
// 정답값 : 1790
// 출력값 : 1780
// A = 8, B = 9
```
- 내 풀이대로면 A가 9를 부여받아야하지만 계산 결과는 답과 거리가 멀다.
- 이유는 A와 B를 같은 수를 부여해놓고 보면 알 수 있는데, A는 100의 자리에 한 번 등장하므로 100, B는 11이 10번 등장하므로 110이 되므로 B가 더 큰 수를 부여받아야 하는 것이다.

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        String[] input = new String[N];
        for (int i = 0; i < N; i++) {
            input[i] = br.readLine();
        }

        // 알파벳의 개수만큼 배열 선언
        int[] alphabet = new int[26];

        // 문자열의 길이-1 만큼 10을 곱한 값을 더해줌!
        for (int i = 0; i < N; i++) {
            int temp = (int) Math.pow(10, input[i].length()-1);

            for (int j = 0; j < input[i].length(); j++) {
                alphabet[input[i].charAt(j) - 65] += temp;
                temp /= 10;
            }
        }

        Arrays.sort(alphabet);
        int numbers = 9;
        int sum = 0;
        
        // 가장 큰 수부터 가장 큰 number를 부여
        for (int i = alphabet.length-1; i >= 0; i--) {
            sum += alphabet[i] * numbers;
            numbers--;
        }

        System.out.println(sum);
    }
}
```
- 위 반례와 같은 문제를 해결하기 위해 자릿수에 맞는 값을 초깃값으로 받고
- (ABBC 라면 A는 1000, B는 110, C는 1이 됨) 
- 초깃값들의 합을 기준으로 가장 큰 수에 9부터 차례로 부여를 하였다!
- [참고 사이트](https://1-7171771.tistory.com/112)