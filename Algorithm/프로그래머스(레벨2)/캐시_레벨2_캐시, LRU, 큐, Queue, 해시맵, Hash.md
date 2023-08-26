# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/17680)

## 📝 문제

지도개발팀에서 근무하는 제이지는 지도에서 도시 이름을 검색하면 해당 도시와 관련된 맛집 게시물들을 데이터베이스에서 읽어 보여주는 서비스를 개발하고 있다.  
이 프로그램의 테스팅 업무를 담당하고 있는 어피치는 서비스를 오픈하기 전 각 로직에 대한 성능 측정을 수행하였는데, 제이지가 작성한 부분 중 데이터베이스에서 게시물을 가져오는 부분의 실행시간이 너무 오래 걸린다는 것을 알게 되었다.  
어피치는 제이지에게 해당 로직을 개선하라고 닦달하기 시작하였고, 제이지는 DB 캐시를 적용하여 성능 개선을 시도하고 있지만 캐시 크기를 얼마로 해야 효율적인지 몰라 난감한 상황이다.

어피치에게 시달리는 제이지를 도와, DB 캐시를 적용할 때 캐시 크기에 따른 실행시간 측정 프로그램을 작성하시오.

### 입력 형식

- 캐시 크기(`cacheSize`)와 도시이름 배열(`cities`)을 입력받는다.
- `cacheSize`는 정수이며, 범위는 0 ≦ `cacheSize` ≦ 30 이다.
- `cities`는 도시 이름으로 이뤄진 문자열 배열로, 최대 도시 수는 100,000개이다.
- 각 도시 이름은 공백, 숫자, 특수문자 등이 없는 영문자로 구성되며, 대소문자 구분을 하지 않는다. 도시 이름은 최대 20자로 이루어져 있다.

### 출력 형식

- 입력된 도시이름 배열을 순서대로 처리할 때, "총 실행시간"을 출력한다.

### 조건

- 캐시 교체 알고리즘은 `LRU`(Least Recently Used)를 사용한다.
- `cache hit`일 경우 실행시간은 `1`이다.
- `cache miss`일 경우 실행시간은 `5`이다.

### 입출력 예제

|캐시크기(cacheSize)|도시이름(cities)|실행시간|
|---|---|---|
|3|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "Jeju", "Pangyo", "Seoul", "NewYork", "LA"]|50|
|3|["Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul"]|21|
|2|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]|60|
|5|["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]|52|
|2|["Jeju", "Pangyo", "NewYork", "newyork"]|16|
|0|["Jeju", "Pangyo", "Seoul", "NewYork", "LA"]|25|

---

### 💡 풀이

- 우선 **LRU 알고리즘은 페이징 기법으로 가장 오랫동안 참조되지 않은 캐시를 교체하는 방법**입니다.
- 새로운 데이터가 들어온 경우,(Miss)
	- 캐시에 넣어줍니다.
	- 캐시가 가득 차있다면 가장 오래된 데이터를 삭제하고 넣어줍니다.
- 존재하는 데이터가 들어온 경우,(Hit)
	- 해당 데이터를 가장 최근 위치로 보내줍니다.
- 위 로직으로 구현해주면 되는 것입니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int cacheSize, String[] cities) {
        int runTime = 0;
        Queue<String> citiesQ = new LinkedList<>();
        
        if (cacheSize == 0) return cities.length * 5;
        
        for (String c : cities) {
            String city = c.toLowerCase();
            
            if (citiesQ.contains(city)) {
                citiesQ.remove(city);
                runTime += 1;
            } else {
                if (citiesQ.size() == cacheSize) {
                    citiesQ.poll();
                }
                runTime += 5;
            }
            citiesQ.add(city);
        }
        
        return runTime;
    }
}
```

- 링크드리스트를 기반으로 한 Queue는 맨 앞 요소의 삭제, 맨 뒤로의 삽입은 빠르지만 contains 메소드와 같은 조회나 중간에 요소를 삽입하고 삭제하는 것은 좋은 편이 아닙니다.
- 그래서 contains 대신 HashMap을 이용해서 containsKey메소드를 이용하는 방법도 생각해보았는데

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJ8luV%2FbtssihfyWsC%2FDFWgkagUqkEbNkWc7iDkI0%2Fimg.png)
<Queue만 사용한 경우>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbB8wdb%2FbtsscCeulbh%2F1UaljVDN5qEl3NtoexU691%2Fimg.png)
<Queue와 HashMap을 사용한 경우>

- 테스트케이스에 따라 성능이 다르고 큰 차이가 없었습니다.
- 아마 캐시 Hit된 경우가 많아서 중간 요소를 조회해서 삭제해야 하는 경우가 많으면 HashMap을 이용하는 경우가 좀 더 나았을 것 같고 Miss가 많다면 맨 앞 요소를 삭제하는 것이므로 Queue의 성능이 좀 더 좋았을 것 같습니다.
- 결국 Queue도 contains와 remove(Object) 메소드의 최적화가 나름 잘 되어있기 때문에 Queue만으로도 통과될 수 있었습니다.