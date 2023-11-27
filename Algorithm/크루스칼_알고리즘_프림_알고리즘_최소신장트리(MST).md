업데이트 예정 : [프림 알고리즘](https://velog.io/@chosj1526/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%94%84%EB%A6%BC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-Prims-Algorithm)

# 최소 신장 트리를 구현하는 알고리즘

# 개념

- 크루스칼 알고리즘과 프림 알고리즘은 그래프 내의 모든 정점들을 최소 비용으로 연결하기 위해 사용
- 그래프 내의 모든 정점을 포함하고 사이클이 없는 연결 선을 그렸을 때 가중치의 합이 최소인 경우

## 신장트리(Spanning Tree)

- 모든 정점을 포함하고 
- 정점 간 서로 연결이 되며 사이클이 존재하지 않는 그래프

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fvlkwz%2Fbtr2aJhW6zV%2F4cQddw1fiwOqpvAjE86o7k%2Fimg.png)
- 5개의 정점과 각 간선들이 선으로 이어져있고 선 위에 가중치가 나타나있다고 가정하면,

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6ASyr%2Fbtr2giK2849%2Fbf7KGS2prilAOUb9kLKWXK%2Fimg.png)
- 위 그림처럼 모든 정점이 연결되어있는 경우가 신장 트리이며
- 아래 그림은 1-2-4 간 사이클이 형성되었기 때문에 신장 트리가 아니다.
- 따라서 **신장 트리의 간선의 개수는 정점-1개**가 된다.
- 그리고 이러한 신장 트리 중 가중치의 합이 최소가 되는 것이 최소 신장 트리(Minimum Spanning Tree, MST)인 것이다!
- 최소 신장 트리는 대표적으로 크루스칼 알고리즘과 프림 알고리즘으로 구현할 수 있다.
- 그래프내 간선이 많은 경우는 프림 알고리즘, 그렇지 않은 경우는 크루스칼 알고리즘이 유리하다고 한다.


## 크루스칼 알고리즘 흐름

- 크루스칼 알고리즘은 그리디 알고리즘의 일종으로, 그래프 간선들을 가중치의 오름차순으로 정렬한 뒤, 사이클을 형성하지 않는 선에서 순서대로 간선을 선택한다.
- 오름차순으로 정렬이 되어있기 때문에 같은 경로이면서 가중치가 다른 경우 혹은 정점간 여러 경로가 있는 경우에 가중치가 작은 경로가 우선 선택이 된다.

> 1. 그래프를 가중치 기준으로 오름차순으로 정렬 (핵심!)
> 2. 정렬된 순서대로 간선을 선택하되, 사이클이 형성되면 건너뛰기
> 3. 간선의 개수가 정점의 개수 - 1이면 종료

![](https://blog.kakaocdn.net/dn/GHGMb/btr2hJ3bepn/lLBVBnRrmDq30UDR0I43H1/img.png)
- 가중치를 기준으로 정렬

![](https://blog.kakaocdn.net/dn/d3ygF0/btr16iSCW3n/GG13HZbCbTpzQIqjpmpQUK/img.png)
- B->D, C->D는 이미 낮은 가중치로 선택된 기존 경로 때문에 사이클 형성이 되므로 이어지지 않음!
- 4개의 정점이므로 간선 4-1개가 되면서 종료


### 사이클 형성 여부 구현

- 사이클이 형성되는지 여부는 **유니온 파인드(Union-Find)** 알고리즘으로 구현하게 된다.
- 유니온 파인드 알고리즘은 선택된 두 노드가 같은 그래프상에 위치하는지(같은 부모를 갖는지)를 구하는 알고리즘인데 같은 부모인 상태에서 Union을 하게 되면 사이클이 형성되므로 이점을 이용하면 되는 것이다!
- 위의 경우 B와 D는 A라는 공통 부모를 가지고 있고 C와 D 또한 공통 부모를 가지고 있기 때문에 연결하지 않게 된다.


## 프림 알고리즘 흐름

- 크루스칼 알고리즘이 가중치를 기준으로 최소인 노드부터 그래프를 형성한다면,
- 프림 알고리즘은 시작 노드를 기준으로 다른 정점들을 하나씩 추가해나가며 그래프를 확장해가는 형식이다.
- 우선순위 큐를 활용한 최소힙을 통해 가장 작은 가중치의 간선을 하나씩 추가하고, 이미 추가된 간선은 건너뛰는 방식으로 구현하게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F8kKQ6%2FbtsAWjoMzZi%2FMCng3t7xbtKsoGsjXWbH10%2Fimg.png)

- 처음에는 모든 노드를 연결하지 않은 상태(visit = false) 로 두고 1번 노드부터 탐색을 시작한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWlJvx%2FbtsAQzApwdT%2FbgQ57yafiTCdU44U1o7W41%2Fimg.png)

- 1번 노드에서 갈 수 있는 코스 중 가장 작은 가중치를 가지고 있는 2번을 연결하고
- 2번에서 갈 수 있는 코스 중 가장 작은 가중치를 다시 연결하며 확장해나간다.
- 4번은 가장 작은 가중치가 2로 1번으로 연결이 되어야 하지만 1번은 이미 방문한 노드이므로 건너뛰고 3번 노드와 연결이 되는 것이다.


### 관련 문제

[크루스칼 알고리즘 풀이 링크](https://github.com/g4dalcom/dev_vault/blob/main/Algorithm/%EB%B0%B1%EC%A4%80(%EA%B3%A8%EB%93%9C4)/%EC%B5%9C%EC%86%8C%20%EC%8A%A4%ED%8C%A8%EB%8B%9D%20%ED%8A%B8%EB%A6%AC(%EB%B0%B1%EC%A4%801197%EB%B2%88)_%EA%B3%A8%EB%93%9C4_%EC%8A%A4%ED%8C%A8%EB%8B%9D%2C%20spanning%20tree%2C%20%EC%8B%A0%EC%9E%A5%20%ED%8A%B8%EB%A6%AC%2C%20%EC%B5%9C%EC%86%8C%20%EC%8B%A0%EC%9E%A5%20%ED%8A%B8%EB%A6%AC%2C%20%ED%81%AC%EB%A3%A8%EC%8A%A4%EC%B9%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98%2C%20MST.md)
[크루스칼 & 프림 알고리즘 풀이 링크](https://github.com/g4dalcom/dev_vault/blob/main/Algorithm/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4(%EB%A0%88%EB%B2%A83)/%EC%84%AC%20%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0_%EB%A0%88%EB%B2%A83_%ED%81%AC%EB%A3%A8%EC%8A%A4%EC%B9%BC_%ED%94%84%EB%A6%BC_%EC%B5%9C%EC%86%8C%EC%8B%A0%EC%9E%A5%ED%8A%B8%EB%A6%AC(MST).md)
