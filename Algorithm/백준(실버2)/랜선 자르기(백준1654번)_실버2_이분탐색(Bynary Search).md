# [문제링크](https://www.acmicpc.net/problem/2805)

## 📝 문제

집에서 시간을 보내던 오영식은 박성원의 부름을 받고 급히 달려왔다. 박성원이 캠프 때 쓸 N개의 랜선을 만들어야 하는데 너무 바빠서 영식이에게 도움을 청했다.

이미 오영식은 자체적으로 K개의 랜선을 가지고 있다. 그러나 K개의 랜선은 길이가 제각각이다. 박성원은 랜선을 모두 N개의 같은 길이의 랜선으로 만들고 싶었기 때문에 K개의 랜선을 잘라서 만들어야 한다. 예를 들어 300cm 짜리 랜선에서 140cm 짜리 랜선을 두 개 잘라내면 20cm는 버려야 한다. (이미 자른 랜선은 붙일 수 없다.)

편의를 위해 랜선을 자르거나 만들 때 손실되는 길이는 없다고 가정하며, 기존의 K개의 랜선으로 N개의 랜선을 만들 수 없는 경우는 없다고 가정하자. 그리고 자를 때는 항상 센티미터 단위로 정수길이만큼 자른다고 가정하자. N개보다 많이 만드는 것도 N개를 만드는 것에 포함된다. 이때 만들 수 있는 최대 랜선의 길이를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에는 오영식이 이미 가지고 있는 랜선의 개수 K, 그리고 필요한 랜선의 개수 N이 입력된다. K는 1이상 10,000이하의 정수이고, N은 1이상 1,000,000이하의 정수이다. 그리고 항상 K ≦ N 이다. 그 후 K줄에 걸쳐 이미 가지고 있는 각 랜선의 길이가 센티미터 단위의 정수로 입력된다. 랜선의 길이는 231-1보다 작거나 같은 자연수이다.

## 출력

첫째 줄에 N개를 만들 수 있는 랜선의 최대 길이를 센티미터 단위의 정수로 출력한다.

## 예제 입력 1 

4 11
802
743
457
539

## 예제 출력 1

200


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
        StringTokenizer st = new StringTokenizer(br.readLine());  
        int K = Integer.parseInt(st.nextToken());  
        int N = Integer.parseInt(st.nextToken());  

		// 배열에 입력값을 담으면서 동시에 max값 구하기!
        int[] lines = new int[K];  
        long max = 0;  
        for (int i = 0; i < K; i++) {  
            lines[i] = Integer.parseInt(br.readLine());  
            if (max < lines[i]) max = lines[i];  
        }  
  
        long min = 0;  
        long mid = 0;  
        while (min <= max) { 
         
            mid = (min + max) / 2;  

			// 중간값으로 몇 개의 랜선이 나오는지 구함!
            long cnt = 0;  
            for (int i = 0; i < lines.length; i++) {  
                cnt += lines[i] / mid;  
            }  

			/**
			* 필요한 랜선 개수(N)와 구한 개수(cnt)를 비교해서
			* 더 적다면 랜선의 길이를 줄여야 개수가 늘어나고
			* 더 많다면 랜선의 길이를 늘려야 한다.
			*/
            if (cnt < N) max = mid - 1;  
            else min = mid + 1;  
        }
        
        System.out.println(max);  
    }  
}
```
- [이분탐색 개념](/Algorithm/이분_탐색(Binary_Search).md)
- 이분 탐색의 기본적인 풀이는 배열 안에서  `특정 인덱스` 를 찾는 것이었다.
- 그러나 이 문제에서는 인덱스가 아니라 `길이` 를 찾아야 하므로 범위는 `길이` 와 `입력값(lines)` 으로 보고 구해야 한다.
	- 중간값(mid)은 (min+max) / 2 인데, 초기 값은 min = 0 이므로 max / 2가 초기 중간값이 된다.
- 그리고 중간값을 구했다면 필요한 랜선의 개수(N)과 비교해서 범위를 좁혀나간다.
