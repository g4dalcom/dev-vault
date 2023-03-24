
## 🔎Test1 : 몇 시간 했더라?
> 경식이는 항해에서 한 주 동안 공부 기록을 남길 알고리즘을 만들어보기로 결심했다.  
> 항해의 체크인 페이지에는 몇가지 조건이 있는데 이를 만족하는 알고리즘을 만들어보자.  
>> 체크인과 체크아웃은 항상 정시에 진행한 것으로 가정한다.  
>> 체크아웃을 할 때 익일 시간은 24+a 로 계산한다. 즉 새벽 2시는 24+2 인 26으로 표기한다.  
>> 체크인 페이지는 체크아웃이 새벽 5시 정각이나 새벽 5시를 넘어가면 체크아웃을 깜빡한 것으로 간주한다.  
>> 따라서 새벽 5시가 넘어가 체크아웃을 하게 되면 자동으로 체크아웃을 오후 9시(21시)로 한 것으로 처리한다.  
>>> 체크인(checkin)과 체크아웃(checkout)을 진행한 시간이 담긴 배열 두 개가 주어진다.  
>>> 각 배열에는 월요일부터 일요일까지 체크인/아웃을 한 시간이 담겨있다.  
>>>checkin과 checkout 배열의 길이는 각각 7 이다.

<br>

```java
public class Main {
    public int solution(int[] arr1, int[] arr2) {
        int answer = 0;
        return answer;
    }

    public static void main(String[] args) {
        Main method = new Main();
        int[] arr1 = {9, 9, 9, 9, 7, 9, 8};
        int[] arr2 = {23, 23, 30, 28, 30, 23, 23};
        System.out.println(method.solution(arr1, arr2));
    }
}
```
<br>

### ✨ 나의 풀이 ✨
1. 기본적으로 arr2[i] - arr1[i] 를 하면 하루동안의 공부시간이 나온다.
2. 이것을 7번 반복하면 일주일 간 총 공부시간이 나온다.
3. 다만, 새벽 5시가 넘어가는 시간(arr2가 29이상일 경우)은 arr2를 21로 치환한다.

<br>

```java
public class Main {
    public int solution(int[] arr1, int[] arr2) {
        int answer = 0;

        for(int i = 0; i < arr1.length; i++) {
        // arr2가 29이상일 경우 21 - arr1을 해준다.
          if(arr2[i] >= 29) {
            answer += 21 - arr1[i];
        // 그렇지 않은 경우는 arr2에서 arr1을 같은 인덱스끼리 마이너스해준다.
          } else {
            answer += arr2[i] - arr1[i];
          }
        }
      
        return answer;
    }

  
    public static void main(String[] args) {
        Main method = new Main();
        int[] arr1 = {9, 9, 9, 9, 7, 9, 8};
        int[] arr2 = {23, 23, 30, 28, 30, 23, 23};
        System.out.println(method.solution(arr1, arr2));
    }
}
```

<br>

#### 조건들도 까다롭지 않고 별다른 어려움 없이 풀 수 있었다!

<br>

## 🔎Test2 : 신대륙 발견
> 예지는 오늘 항해99를 시작했다.  
> 성격이 급한 예지는 항해 1일 차부터 언제 수료를 하게될 지 궁금하다.  
> 항해 1일 차 날짜를 입력하면 98일 이후 항해를 수료하게 되는 날짜를 계산해주는 알고리즘을 만들어보자.  
>> 1 ≤ month ≤ 12  
>> 1 ≤ day ≤ 31 (2월은 28일로 고정합니다, 즉 윤일은 고려하지 않습니다.)  

<br>

```java
public class Main {
    public String solution(int month, int day) {
        String answer = "";
        return answer;
    }

    public static void main(String[] args) {
        Main method = new Main();
        System.out.println(method.solution(1, 18));
    }
}
```

<br>


### ✨ 나의 풀이 ✨
```java
int[] months = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};
int[] date = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
```
1. 우선 months라는 1~12월까지를 적어놓은 배열과 date라는 각 월의 총 일수를 적은 배열 두 가지를 선언해두었다.
2. 주어지는 일자를 6월 22일이라고 가정한다.  
3. 주어진 일(22일)에 98일을 더한다. > 120일  
4. 6월과 같은 인덱스(5번 인덱스)인 date[5]의 value를 120일에서 마이너스한다. > 90일
5. 인덱스를 +1해서 7월의 일수를 마이너스한다. > 59일
6. 8월의 일수를 마이너스한다. > 28일
7. 9월의 일수로 마이너스할 수 없으므로 멈춘다.
8. 8월의 일수까지 마이너스 했으므로 월은 9월이 되고 일은 최종적으로 남은 28일이 된다.

<br>

```java
public class Main {
    public String solution(int month, int day) {
        String answer = "";

        int[] months = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};
        int[] date = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

        int days = day + 98;   // 주어진 일에 98 더하기
        int idx = month - 1;   // 인덱스 변수 선언, 인덱스는 실제 월수 -1이므로(1월이면 0번 인덱스이므로) -1을 해준다.
        int mon = month;       // 인덱스가 1 변화하는 만큼 월도 1 변화되어야 하므로 따로 변수를 선언해준다.
          
        while(days > date[idx]) {         // 일에 98 더한 수가 다음 date인덱스보다 큰 동안 아래를 수행한다.
          days -= date[idx];              // days에 처음 주어진 월의 인덱스과 같은 date 인덱스의 value를 마이너스해준다.
          idx++;                          // 그 후에 인덱스를 1 증가
          mon++;                          // 그리고 월도 1 증가
          if(idx == 12) {                 // 만약에 idx가 12가 된다면, 총 인덱스의 길이는 11이므로(12월 다음에 1월로 넘어가는 조건)
           idx = 0;                       // 인덱스를 0으로 초기화한다.(1월의 인덱스로 넘어옴)
           mon = 1;                       // 월은 1월로 초기화한다.(12월에서 1월로 넘어옴)
          }
        } answer = mon+"월 "+days+"일";
        
        return answer;
    }

  
    public static void main(String[] args) {
        Main method = new Main();
        System.out.println(method.solution(1, 18));
    }
}
```

- 어떻게 풀어나가야 할지 감을 못잡았던 문제이다.  
- 이리저리 더해보고 하다가 우연히 주어진 일에 98을 더했고  
- 그 후에 30 밑으로 내려갈 때까지 뺐더니 예제의 정답과 같은 일이 나오길래  대략 이런식으로 접근하면 되겠구나 하고 깨달았다.  
- 문제를 단순화해서(단순히 30 밑으로 내려갈 때까지 뺐다던가) 접근한 후에 실제로 코드를 작성하면서 구체화했던 것이 주요했던 것 같다.  
- 접근방법을 알아낸 후에 코드를 작성하는 것은 비교적 쉬웠고 머릿속으로 모든 것을 그린 후에 코드 작성에 들어가는 것이 아니라  코드작성을 하면서 필요한 변수를 추가적으로 선언하고 코드를 수정해가며 완성시킬 수 있었다.  
- 처음에는 idx 변수도 없었고 mon 변수도 없이 month 에 그대로 값을 저장하다보니 오류가 있었으나 작성하면서 수정할 수 있었다.  
- 달력 문제는 알고리즘 연습할 때에도 풀지 못했고 두려웠던 문제인데  이 문제를 풀면서 자신감이 조금 생겼다고 할 수 있겠다.  