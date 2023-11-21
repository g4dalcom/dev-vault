# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/43164)

## 📝 문제

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

##### 입출력 예

|tickets|return|
|---|---|
|[["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]|["ICN", "JFK", "HND", "IAD"]|
|[["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]]|["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"]|

##### 입출력 예 설명

예제 #1

["ICN", "JFK", "HND", "IAD"] 순으로 방문할 수 있습니다.

예제 #2

["ICN", "SFO", "ATL", "ICN", "ATL", "SFO"] 순으로 방문할 수도 있지만 ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] 가 알파벳 순으로 앞섭니다.

---

### 💡 풀이

- 처음에 가장 먼저 생각했던 풀이법은 해시맵을 이용한 것이었습니다. 
- 출발지를 key값으로, 도착지를 우선순위큐를 이용한 value로 설정을 하고 우선순위큐는 사전순으로 우선순위를 갖도록 하였습니다.
- 그리고 "ICN" 부터 큐에서 하나씩 뺀 후, 해당 도시의 value를 해시맵에서 불러온 후 큐에서 다음 목적지를 빼내는 식이었습니다.

### ❌ 실패 케이스(해시맵)

```java
import java.util.*;

class Solution {
    public String[] solution(String[][] tickets) {
        HashMap<String, PriorityQueue<String>> map = new HashMap<>();
        PriorityQueue<String> pq;
        
        for (String[] ticket : tickets) {
            String departure = ticket[0];
            String arrival = ticket[1];
            
            if (map.containsKey(departure)) {
                pq = map.get(departure);
                pq.offer(arrival);
            } else {
                pq = new PriorityQueue<>((o1, o2) -> o1.compareTo(o2));
                pq.offer(arrival);
            }
            map.put(departure, pq);
        }
        
        ArrayList<String> routes = new ArrayList<>();
        String route = "ICN";
        routes.add(route);
        
        while (map.size() > 0) {
            PriorityQueue<String> current = map.get(route);
            String temp = current.poll();
            routes.add(temp);
            
            if (current.isEmpty()) {
                map.remove(route);
            }
            
            route = temp;
        }

        return routes.stream().toArray(String[]::new);
    }
}
```

- 이 방법은 아래와 같은 케이스에서 오류가 발생합니다.
- tickets : \[\["ICN", "JFK"], \["ICN", "AAD"\], \["JFK", "ICN"\]\]
- return : \["ICN", "JFK", "ICN", "AAD"\]\
- 단순히 출발지가 같을 경우 목적지를 사전순으로 정렬하는 게 아니라 **여러 개의 경로로 모든 티켓을 사용할 수 있을 때** 사전순으로 정렬을 해야 했습니다.
- 따라서 ICN 같은 경우 AAD와 JFK 중 AAD가 사전순으로 우선인 목적지이지만 AAD를 먼저 갈 경우 모든 티켓을 사용할 수 없기 때문에 우선순위큐를 이용한 단순 풀이법은 사용할 수 없고 **완전 탐색**을 해야만 합니다.
- 두 번째 풀이는 클래스를 이용한 풀이였습니다. City 클래스에 해당 도시가 갈 수 있는 목적지를 담고 있는 ArrayList\<String\> 와 완전 탐색시 방문했던 목적지를 표시하기 위한 ArrayList\<Boolean\> 을 선언하고 완전 탐색을 시도하였는데요...

### ❌ 실패 케이스

```java
import java.util.*;

class Solution {
    static HashMap<String, City> map;
    static String[] routes;
    static String[] result;
    static boolean isComplete = false;
    
    public String[] solution(String[][] tickets) {
        map = new HashMap<>();
        for (String[] ticket : tickets) {
            City city;
            if (map.containsKey(ticket[0])) {
                city = map.get(ticket[0]);;
            } else {
                city = new City();
            }
            city.setArrival(ticket[1]);
            map.put(ticket[0], city);
        }
        
        routes = new String[tickets.length+1];
        routes[0] = "ICN";
        City start = map.get("ICN");
        dfs(start, routes.length, 1);
        
        return result;
    }
    
    public void dfs(City start, int size, int depth) {
        if (size == depth) {
            result = Arrays.stream(routes).toArray(String[]::new);
            isComplete = true;
            return;
        };
        
        for (int i = 0; i < start.arrival.size(); i++) {
            if (!isComplete && !start.visit.get(i)) {
                start.setVisit(i, true);
                routes[depth] = start.arrival.get(i);
                dfs(map.get(start.arrival.get(i)), size, depth+1);
                start.setVisit(i, false);
            }
        }
    }
    
    public class City {
        ArrayList<Boolean> visit;
        ArrayList<String> arrival;
        
        public City() {
            this.visit = new ArrayList<>();
            this.arrival = new ArrayList<>();
        }
        
        public void setVisit(int index, boolean bool) {
            this.visit.set(index, bool);
        }
        
        public void setArrival(String city) {
            this.arrival.add(city);
            this.visit.add(false);
            Collections.sort(arrival);
        }
    }
}
```

- 목적지를 추가할 때마다 visit도 추가해주면서 나름 신경써서 클래스를 구성했고 도착지를 사전순으로 우선 정렬하여 dfs 탐색시 먼저 탐색되도록 하였습니다.
- 이 방법은 출발지와 목적지가 같은 티켓이 여러 개 존재할 경우, arrival 리스트를 정렬하는 과정에서 인덱스가 섞이며 런타임 오류를 발생시켰습니다.
- 이 과정에서 깨달은 점은, 각각의 출발지에 목적지 리스트를 담아서 해결할 것이 아니라 입력값으로 주어지는 티켓 자체를 가지고 해결해야 한다는 점이었습니다.
- 이 점을 가지고 접근해보니 오히려 훨씬 간단하게 풀이를 할 수 있었습니다.
- 우선은 tickets 리스트를 사전순으로 정렬을 합니다. dfs 탐색시 올바른 경로를 가장 먼저 탐색하기 위함입니다.
- 그리고 리턴되는 배열의 길이는 항상 tickets.length+1이므로 dfs의 탐색 깊이가 tickets.length+1이 된다면 모든 티켓을 사용한 올바른 경로라고 확신할 수 있습니다. 이 이후로는 탐색을 하지 않아도 되므로 isUsedTicket 값으로 탐색을 돌지 않도록 하였습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    static String[] routes;
    static boolean[] isUsedTicket;
    static boolean isComplete = false;
    
    public String[] solution(String[][] tickets) {
        routes = new String[tickets.length+1];
        isUsedTicket = new boolean[tickets.length];
        
        Arrays.sort(tickets, (String[] o1, String[] o2) -> o1[0].equals(o2[0]) ? o1[1].compareTo(o2[1]) : o1[0].compareTo(o2[0]));
        
        routes[0] = "ICN";
        dfs("ICN", 1, tickets);
        
        return routes;
    }
    
    public void dfs(String current, int depth, String[][] tickets) {
        if (depth == tickets.length+1) {
            isComplete = true;
            return;
        }
        
        for (int i = 0; i < tickets.length; i++) {
            if (!isComplete && !isUsedTicket[i] && current.equals(tickets[i][0])) {
                routes[depth] = tickets[i][1];
                isUsedTicket[i] = true;
                dfs(tickets[i][1], depth+1, tickets);
                isUsedTicket[i] = false;
            }
        }
    }
}
```




