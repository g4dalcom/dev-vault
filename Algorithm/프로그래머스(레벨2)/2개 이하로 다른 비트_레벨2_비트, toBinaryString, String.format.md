# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/77885)

## 📝 문제

양의 정수 `x`에 대한 함수 `f(x)`를 다음과 같이 정의합니다.

- `x`보다 크고 `x`와 **비트가 1~2개 다른** 수들 중에서 제일 작은 수

예를 들어,

- `f(2) = 3` 입니다. 다음 표와 같이 2보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 3이기 때문입니다.

|수|비트|다른 비트의 개수|
|---|---|---|
|2|`000...0010`||
|3|`000...0011`|1|

- `f(7) = 11` 입니다. 다음 표와 같이 7보다 큰 수들 중에서 비트가 다른 지점이 2개 이하이면서 제일 작은 수가 11이기 때문입니다.

|수|비트|다른 비트의 개수|
|---|---|---|
|7|`000...0111`||
|8|`000...1000`|4|
|9|`000...1001`|3|
|10|`000...1010`|3|
|11|`000...1011`|2|

정수들이 담긴 배열 `numbers`가 매개변수로 주어집니다. `numbers`의 모든 수들에 대하여 각 수의 `f` 값을 배열에 차례대로 담아 return 하도록 solution 함수를 완성해주세요.

---

##### 제한사항

- 1 ≤ `numbers`의 길이 ≤ 100,000
- 0 ≤ `numbers`의 모든 수 ≤ 1015

---

##### 입출력 예

|numbers|result|
|---|---|
|`[2,7]`|`[3,11]`|

---

### 💡 풀이

- 숫자를 하나씩 늘려가면서 2진수로 바꾸고 비교하는 방식은 마지막 두 개의 테스트케이스에서 시간 초과가 발생합니다.
- 비트 연산을 이용한 방법도 찾아서 해보았는데 역시나 시간 초과가 발생해서 다른 풀이를 보고 힌트를 얻어서 풀 수 있었습니다 ㅠㅠ
- 우선 비트에서의 규칙성을 찾아야 했는데요!
- 일의 자리가 0과 1로 번갈아나타나는 점에서 짝수와 홀수를 구분할 수 있습니다. 짝수의 경우는 일의 자리가 0이고 홀수는 1이 되죠?! 이 때 짝수에서 홀수로 바뀌는 경우는 일의 자리만 달라지므로 **x보다 크면서 x와 비트가 2개 이하로 다른 수**라는 조건에 부합됩니다. 따라서 짝수인 경우는 +1인 값이 조건에 맞는 값이 되겠네요!

```java
if (numbers[i] % 2 == 0) {
	result[i] = numbers[i] + 1;
}
```

- 문제는 홀수인데 홀수의 경우는 크게 두 가지 경우의 수가 있습니다.
- 1111 처럼 모든 수가 1로 이루어져있는 경우, 이 경우에는 1이 증가하면 10000 이 됩니다. 자리수가 늘어나고 뒤에 이어지는 모든 수가 0이 되지요. 하지만 이 경우는 **x보다 크면서 x와 비트가 2개 이하로 다른 수** 에 부합하지 않습니다. 1111보다 크면서 비트가 2개 이하로 다르려면 10111, 11011, 11101, 11110 의 경우밖에 없는데 이 중 가장 작은 수는 10111 입니다. 1111이 10111이 되려면 앞에 **10**이 붙고 **length() - 1만큼 1**을 붙여주면 된다고 생각했습니다.

```java
public String consistOfOnlyOne(StringBuilder sb, int length) {
	sb.append("10");
	
	while (length-- > 1) {
		sb.append("1");
	}
	
	return sb.toString();
}
```

- 또 다른 경우는 10011처럼 1과 0이 혼합되어 있는 경우인데요! 이 때 1을 증가시키면 10100이 되면서 **x보다 크면서 x와 비트가 2개 이하로 다른 수**에 부합되지 못하게 됩니다. 가장 가까운 수는 10101이 되는 것이죠. 여기서도 규칙을 찾아보면 뒤에서부터 0이 처음 등장한 곳과 다음 수를 **10**으로 치환해주는 것과 같다는 것을 알 수 있었습니다.

```java
public String mixedTogether(StringBuilder sb, String toBinary) {
	int index = toBinary.lastIndexOf("0");
	sb.append(toBinary.substring(0, index));
	sb.append("10");
	sb.append(toBinary.substring(index+2));
	
	return sb.toString();
}
```

- 진법 계산이나 비트 연산이 익숙하지 않아서 처음 시행착오를 겪을 때에도 **toBinaryString()** 이나 **String.format()** 과 같은 메소드를 공부할 수 있었습니다. 비록 방향을 맞지 않았지만요...!
- 힌트를 얻은 후에도 규칙을 찾아서 구현하는 데 꽤나 오랜 시간이 걸렸습니다 ㅠㅠ 2진수에 익숙하지 않았던 탓인 거 같습니다.


### 🔍 정답

```java
class Solution {
    public long[] solution(long[] numbers) {
        long[] result = new long[numbers.length];
        
        for (int i = 0; i < numbers.length; i++) {
            if (numbers[i] % 2 == 0) {
                result[i] = numbers[i] + 1;
                continue;
            }
            
            String toBinary = Long.toBinaryString(numbers[i]);
            StringBuilder sb = new StringBuilder();
            String compare = "";
            
            if (toBinary.indexOf("0") == -1) {
                int length = toBinary.length();
                compare = consistOfOnlyOne(sb, length);        
            } else {
                compare = mixedTogether(sb, toBinary);
            }
            
            result[i] = Long.parseLong(compare, 2);
        }
        
        return result;
    }
    
    public String consistOfOnlyOne(StringBuilder sb, int length) {
        sb.append("10");
        
        while (length-- > 1) {
            sb.append("1");
        }
        
        return sb.toString();
    }
    
    public String mixedTogether(StringBuilder sb, String toBinary) {
        int index = toBinary.lastIndexOf("0");
        sb.append(toBinary.substring(0, index));
        sb.append("10");
        sb.append(toBinary.substring(index+2));
        
        return sb.toString();
    }
}
```


### ❌ 시행착오

```java
class Solution {
    public long[] solution(long[] numbers) {
        long[] result = new long[numbers.length];
        
        for (int i = 0; i < numbers.length; i++) {
            long current = numbers[i];
            String currentToBinary = toBinaryString(current);
            
            while (true) {
                String comparison = toBinaryString(++current);
                
                if (compareBinary(currentToBinary, comparison)) break;
            }
            
            result[i] = current;
        }

        return result;
    }
    
    public String toBinaryString(long number) {
        return Long.toBinaryString(number);
    }
    
    public boolean compareBinary(String current, String comparison) {
        int count = 0;
        boolean flag = true;
        
        int currentPointer = current.length()-1;
        int comparisonPointer = comparison.length()-1;
        
        char o1 = current.charAt(currentPointer);
        char o2 = comparison.charAt(comparisonPointer);
        
        while (currentPointer >= 0 || comparisonPointer >= 0) {
            if (o1 != o2) count++;
            
            if (count > 2) {
                flag = false;
                break;
            }
            
            currentPointer--;
            comparisonPointer--;
            
            o1 = currentPointer < 0 ? '0' : current.charAt(currentPointer);
            o2 = comparisonPointer < 0 ? '0' : comparison.charAt(comparisonPointer);
        }
        
        return flag;
    }
}
```


### ❌ 시행착오(비트 연산)

```java
class Solution {
    public long[] solution(long[] numbers) {
        long[] result = new long[numbers.length];
        
        for (int i = 0; i < numbers.length; i++) {
            long current = numbers[i];
            String currentToBinary = toBinaryString(current);
            
            while (true) {
                String comparison = toBinaryString(++current);
                
                if (compareBinary(currentToBinary, comparison)) break;
            }
            
            result[i] = current;
        }

        return result;
    }
    
    public String toBinaryString(long number) {
        StringBuilder result = new StringBuilder();

        for (int i = 63; i >= 0 ; i--) {
            long mask = (number >> i) & 1;
            result.append(mask);
        }
        
        return result.toString();
    }
    
    public boolean compareBinary(String current, String comparison) {
        int count = 0;

        for (int i = current.length()-1; i >= 0; i--) {
            if (current.charAt(i) != comparison.charAt(i)) count++;
            if (count > 2) return false;
        }
        
        return true;
    }
}
```