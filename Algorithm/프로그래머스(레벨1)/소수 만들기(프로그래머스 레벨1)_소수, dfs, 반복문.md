# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12977)

## 📝 문제

주어진 숫자 중 3개의 수를 더했을 때 소수가 되는 경우의 개수를 구하려고 합니다. 숫자들이 들어있는 배열 nums가 매개변수로 주어질 때, nums에 있는 숫자들 중 서로 다른 3개를 골라 더했을 때 소수가 되는 경우의 개수를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

-   nums에 들어있는 숫자의 개수는 3개 이상 50개 이하입니다.
-   nums의 각 원소는 1 이상 1,000 이하의 자연수이며, 중복된 숫자가 들어있지 않습니다.

---

##### 입출력 예

| nums            | result |
|:--------------- |:------ |
| [1, 2, 3, 4]    | 1      |
| [1, 2, 7, 6, 4] | 4       |


##### 입출력 예 설명

입출력 예 #1  
[1,2,4]를 이용해서 7을 만들 수 있습니다.

입출력 예 #2  
[1,2,4]를 이용해서 7을 만들 수 있습니다.  
[1,4,6]을 이용해서 11을 만들 수 있습니다.  
[2,4,7]을 이용해서 13을 만들 수 있습니다.  
[4,6,7]을 이용해서 17을 만들 수 있습니다.

---

### 🔍 정답

```java
class Solution {
    
    static int cnt = 0;
    static boolean[] visit;
    static int sum;
    static boolean[] notPrime;
    
    public int solution(int[] nums) {
        int NUM = 500001;
        
        // 소수 판별(소수 아닌 뇨속들은 true)
        notPrime = new boolean[NUM];
        for (int i = 2; i < NUM; i++) {
            for (int j = 2; i*j < NUM; j++) {
                notPrime[i*j] = true;
            }
        }

        // 백트래킹 시작!
        visit = new boolean[nums.length];
        for (int i = 0; i < nums.length; i++) {
            sum = nums[i];
            visit[i] = true;
            dfs(i, 1, nums);
        }

        return cnt;
    }

    public static void dfs(int num, int depth, int[] nums) {

        // 3개 더해서 소수인지 여부 확인
        if (depth == 3) {
            if (!notPrime[sum]) {
                cnt++;
            }
            return;
        }

        for (int i = num; i < nums.length; i++) {
            if (!visit[i]) {
                visit[i] = true;
                sum += nums[i];
                dfs(i, depth+1, nums);
                sum -= nums[i];
                visit[i] = false;
            }
        }
    }
}
```


### 💡 다른 풀이

```java
class Solution {

    public int solution(int[] nums) {
        int ans = 0;

        for(int i = 0; i < nums.length - 2; i ++){
            for(int j = i + 1; j < nums.length - 1; j ++){
                for(int k = j + 1; k < nums.length; k ++ ){
                    if(isPrime(nums[i] + nums[j] + nums[k])){
                        ans += 1;  
                    } 
                }
            }
        }
        return ans;
    }
    public Boolean isPrime(int num){
        int cnt = 0;
        for(int i = 1; i <= (int)Math.sqrt(num); i ++){
            if(num % i == 0) cnt += 1; 
        }
        return cnt == 1;
    }
}
```