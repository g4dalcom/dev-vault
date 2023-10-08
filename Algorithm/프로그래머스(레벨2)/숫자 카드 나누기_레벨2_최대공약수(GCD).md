# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/135807)

## 📝 문제

철수와 영희는 선생님으로부터 숫자가 하나씩 적힌 카드들을 절반씩 나눠서 가진 후, 다음 두 조건 중 하나를 만족하는 가장 큰 양의 정수 a의 값을 구하려고 합니다.

1. 철수가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고 영희가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a
2. 영희가 가진 카드들에 적힌 모든 숫자를 나눌 수 있고, 철수가 가진 카드들에 적힌 모든 숫자들 중 하나도 나눌 수 없는 양의 정수 a

예를 들어, 카드들에 10, 5, 20, 17이 적혀 있는 경우에 대해 생각해 봅시다. 만약, 철수가 [10, 17]이 적힌 카드를 갖고, 영희가 [5, 20]이 적힌 카드를 갖는다면 두 조건 중 하나를 만족하는 양의 정수 a는 존재하지 않습니다. 하지만, 철수가 [10, 20]이 적힌 카드를 갖고, 영희가 [5, 17]이 적힌 카드를 갖는다면, 철수가 가진 카드들의 숫자는 모두 10으로 나눌 수 있고, 영희가 가진 카드들의 숫자는 모두 10으로 나눌 수 없습니다. 따라서 철수와 영희는 각각 [10, 20]이 적힌 카드, [5, 17]이 적힌 카드로 나눠 가졌다면 조건에 해당하는 양의 정수 a는 10이 됩니다.

철수가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 `arrayA`와 영희가 가진 카드에 적힌 숫자들을 나타내는 정수 배열 `arrayB`가 주어졌을 때, 주어진 조건을 만족하는 가장 큰 양의 정수 a를 return하도록 solution 함수를 완성해 주세요. 만약, 조건을 만족하는 a가 없다면, 0을 return 해 주세요.

---

##### 제한사항

제한사항

- 1 ≤ `arrayA`의 길이 = `arrayB`의 길이 ≤ 500,000
- 1 ≤ `arrayA`의 원소, `arrayB`의 원소 ≤ 100,000,000
- `arrayA`와 `arrayB`에는 중복된 원소가 있을 수 있습니다.

---

##### 입출력 예

|arrayA|arrayB|result|
|---|---|---|
|[10, 17]|[5, 20]|0|
|[10, 20]|[5, 17]|10|
|[14, 35, 119]|[18, 30, 102]|7|

---

##### 입출력 예 설명

**입출력 예 #1**

- 문제 예시와 같습니다.

**입출력 예 #2**

- 문제 예시와 같습니다.

**입출력 예 #3**

- 철수가 가진 카드에 적힌 숫자들은 모두 3으로 나눌 수 없고, 영희가 가진 카드에 적힌 숫자는 모두 3으로 나눌 수 있습니다. 따라서 3은 조건에 해당하는 양의 정수입니다. 하지만, 철수가 가진 카드들에 적힌 숫자들은 모두 7로 나눌 수 있고, 영희가 가진 카드들에 적힌 숫자는 모두 7로 나눌 수 없습니다. 따라서 최대값인 7을 return 합니다.

---

### 💡 풀이

- 최대공약수를 이용한 풀이라는 것은 빠르게 알 수 있었습니다.
- 수들을 공통으로 나눌 수 있는 수 중 가장 큰 수 = 최대공약수
- 처음 생각했던 것은 최대공약수의 약수들, 즉 수들을 나눌 수 있는 공약수들의 집합으로 반대편 수들이 하나도 나누어떨어지지 않는 가장 큰 수를 찾아야 한다고 생각하였습니다.
- 그래서 최대공약수를 구하고, 공약수들을 구하고, 공약수들과 반대편 배열의 수들을 비교하는 식으로 구현을 했다가 손으로 직접 여러 경우의 수를 써본 결과 공약수들과 비교하지 않고 최대공약수만으로 비교해도 된다는 결론을 얻었습니다.
- 예를 들어서 A = \[10, 20\], B = \[5, 17\] 일 때, A의 최대 공약수는 10이 됩니다. 처음 생각했던 대로라면 10의 약수인 2, 5, 10을 가지고 B와 비교하는 식이었고 그렇게 생각한 이유는 반대편 수들 중 10으로 나누어떨어지는 수가 존재할 수 있고 그렇다면 더 작은 수를 탐색해야 한다고 생각했기 때문입니다.
- 그러나 10으로 나누어떨어진다면 당연히 더 작은 수인 2, 5로도 나누어떨어지기 때문에 최대공약수만 비교해도 되는 문제였습니다.


### 🔍 정답

```java
class Solution { 
    public int solution(int[] arrayA, int[] arrayB) {
        int GCD_A = 0;
        int GCD_B = 0;

        for (int i = 0; i < arrayA.length; i++) {
            GCD_A = GCD(arrayA[i], GCD_A);
            GCD_B = GCD(arrayB[i], GCD_B);
        }

        if (!divide(GCD_A, arrayB) && !divide(GCD_B, arrayA)) return 0;
        else return Math.max(GCD_A, GCD_B);
    }

    public int GCD(int a, int b) {
        if (b == 0) return a;
        else if (b == 1) return 1;

        return GCD(b, a % b);
    }

    public boolean divide(int GCD, int[] array) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] % GCD == 0) return false;
        }

        return true;
    }
}
```


### ❌ 시행착오

```java
import java.util.*;

class Solution { 
    static int max = 0;
    
    public int solution(int[] arrayA, int[] arrayB) {
        int GCD_A = 0;
        int GCD_B = 0;
        
        for (int i = 0; i < arrayA.length; i++) {
            GCD_A = GCD(arrayA[i], GCD_A);
            GCD_B = GCD(arrayB[i], GCD_B);
        }
        
        ArrayList<Integer> commonDivisor_A = new ArrayList<>();
        ArrayList<Integer> commonDivisor_B = new ArrayList<>();
        
        divisor(GCD_A, commonDivisor_A);
        divisor(GCD_B, commonDivisor_B);
        
        isPossibleDivide(arrayA, commonDivisor_B);
        isPossibleDivide(arrayB, commonDivisor_A);

        return max;
    }
    
    public int GCD(int a, int b) {
        if (b == 0) return a;
        else if (b == 1) return 0;
        
        return GCD(b, a % b);
    }
    
    public void divisor(int GCD, ArrayList<Integer> commonDivisor) {
        for (int i = 2; i <= Math.sqrt(GCD); i++) {
            if (GCD % i == 0) {
                if (GCD % i != i) {
                    commonDivisor.add(i);
                } 
                commonDivisor.add(GCD / i);
            }
        }
        if (GCD != 0 ) commonDivisor.add(GCD);
    }
    
    public void isPossibleDivide(int[] array, ArrayList<Integer> commonDivisor) {
        Collections.reverse(commonDivisor);
        
        for (int divisor : commonDivisor) {
            boolean flag = false;
            for (int element : array) {
                if (element % divisor == 0) {
                    flag = true;
                }
            }
            
            if (!flag) {
                max = Math.max(max, divisor);
                break;
            }
        }
    }
}
```