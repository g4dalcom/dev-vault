# 그리디 알고리즘

- 그리디 알고리즘(탐욕법)은 현재 상황에서 지금 당장 좋은 것만 고르는 방법을 의미합니다.
- 일반적인 그리디 알고리즘은 문제를 풀기 위한 최소한의 아이디어를 떠올릴 수 있는 능력을 요구합니다.
- 그리디 해법은 그 정당성 분석이 중요합니다.
    - 단순히 가장 좋아보이는 것을 반복적으로 선택해도 최적의 해를 구할 수 있는지 검토합니다.

[문제] 루트 노드부터 시작하여 거쳐가는 노드 값의 합을 최대로 만들고 싶습니다.

- Q. 최적의 해는 무엇인가요?

![](https://lh6.googleusercontent.com/ecmAqkA1zJnG__0nFnyeTtLnPDBZPMtKYQmlpnf2HwsYLT1s1Sq2aYZHw74vyIHfQiaO_cGLfjfMP36NTBUoron6q3Z9Xvs85y-RFTXAPmN0cAeM1r6roybhXlvKu3tRqLht5CAgiVM=s1600)

- Q. 단순히 매 상황에서 가장 큰 값만 고른다면 어떻게 될까요?(그리디 방식)

![](https://lh5.googleusercontent.com/LjP9xw8dAjrWS6ePyxQfiHZt-PGm2Pv8pOCyr-zIuEZibfpxV2aVA6WBoxh4jLAdAP_Jd5mCrbRIZ5kikYCWjPJo4xsh6DRAvTL-NTFeY27Zo_P4jnzVgJNxJysufuFDOj4Y_0eDoD0=s1600)

그리디 알고리즘은 이처럼 매 상황에서 가장 큰 값을 찾아가기는 방식이다.

- 일반적인 상황에서 그리디 알고리즘은 최적의 해를 보자알 수 없을 때가 많습니다.
- 하지만 코딩 테스트에서의 대부분의 그리디 문제는 탐욕법으로 얻은 해가 최적의 해가 되는 상황에서, 이를 추론할 수 있어야 풀리도록 출제됩니다.

# [문제] 거스름 돈

## 문제 설명

- 당신은 음식점의 계산을 도와주는 점원입니다. 카운터에는 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리 동전이 무한히 존재한다고 가정합니다. 손님에게 거슬러 주어야 할 돈이 N원일 때, 거슬러 주어야 할 동전의 최소 개수를 구하세요. 단, 거슬러 줘야할 돈 N은 항상 10의 배수입니다.(즉, 거슬러주지 못할 경우 없음)

![](https://lh6.googleusercontent.com/y5uAKuJ7wKIR585z9WE2FWX5TTWqRdVydo6LyEFb4t7U9fV4q1qmcbe-E2NlVVfxwAx0mz3_8V1TcVw_J19_VRFJLnzddYVcW_WSthL69A-dnKFktMQWjJXZf3J9xb-jHmZmrWn91QQ=s1600)

## 문제 해결 아이디어

- 최적의 해를 빠르게 구하기 위해서는 **가장 큰 화폐단위 부터** 돈을 거슬러 주면 됩니다.
- N원을 거슬러 줘야할때, 가장 먼저 500원으로 거슬러 줄 수 있을 만큼 거슬러 줍니다.
    - 이후에 100원, 50원, 10원짜리 동전을 차례대로 거슬러 줄 수 있을 만큼 거슬러 주면 됩니다.
- N = 1,260원일 때의 예시를 확인해 봅시다.

### [Step 0] 남은 돈: 1,260원(원래 점원은 무한히 많은 동전의 개수를 가지고 있음)

![](https://lh5.googleusercontent.com/EVPJZUQLdaoOiCzHaSwT7YxnQRqtctMWOLAdWtmsg3ZgwOXbUGYkKTyKQ80BBgTwDgPmtbsv1XYb0msMsxZZqrClQk8eRMgWj43y5P0-mWvf9xdOXjVuBceF7Z_s7T26ds8ZggqNlgA=s1600)

### [Step 1] 남은 돈: 260원

![](https://lh6.googleusercontent.com/xfHPz-bKHuR6ikUY-AX0L8YaPvE-3RL8R8Ch3v_ylBVodxtX_pe9WUST6rEiZQTvKIfSHO4uEI7sVAOgtje7BuFSF9cEXTwqu7_5PGX-VfzCZ9-JDs61AKNjDw3SbexxgGrXv-RxFTY=s1600)

### [Step 2] 남은 돈: 60원

![](https://lh5.googleusercontent.com/EAwr48rM2LBd4Fu40FLdlKnBtHMW6WwSeAsbIR14u0UdG1Q-IG8seibUOdJ5QlmJV2nQ9skU0Ebtuj3S2l8ZhsB-IWEC9TIqs3TebGA_VM1jGMLmmOHTNV4c1uoEIxHi33zglCuR9eM=s1600)

### [Step 3] 남은 돈: 10원

![](https://lh5.googleusercontent.com/KCw6x5JpAMNpobR3F3WqtHntXBzTZcK5yEQGM35r_RiIL0PLsOCI9CLzNtYP7A1XLZcLzBLBz5eiUBTqrq3_V9iCYsWxUGFqUKEnvXvWcSGs1a9CfwFPP4QLNQ1KEqppjbkiFmd5gOU=s1600)

### [Step 4] 남은 돈: 0원

![](https://lh6.googleusercontent.com/ON67EDUcoiebeyiWMN2kLc_qWVNu0fZMcoPQPrYBAqfIStRrJcm4ilroN9U6gZgIBuvM7-VUMa85bqYeoMSd5WO9YGdpjm5oouUHerz-KUWQXfnm0M3aiGqtfPxwZ0sfbcA27MM_l1w=s1600)

## 정당성 분석

- 가장 큰 화폐 단위부터 돈을 거슬러 주는 것이 최적의 해를 보장하는 이유는 무엇일까요?
    - 가지고 있는 동전 중에서 **큰 단위가 항상 작은 단위의 배수이므로 작은 단위의 동전들을 종합해 다른 해가 나올 수 없기 때문**입니다.
- 예시)만약에 800원을 거슬러 주어야 하는데 화폐 단위가 500원, 400원, 100원이라면 어떻게 될까요?
    - 그리디 방식이라면 500원-1개, 100원-3개=> 총 4개이지만, 400원-2개 => 총 2개가 최적의 해이다.(400원이 500원의 배수가 아니기 때문에 발생한 문제)
- 그리디 알고리즘 문제에서는 이처럼 문제 풀이를 위한 최소한의 아이디어를 떠올리고 이것이 정당한지 검토할 수 있어야 합니다.

# 거스름 돈 : 답안

```python
n = 1260
count = 0

# 큰 단위의 화폐부터 차례대로 확인하기
array = [500, 100, 50, 10]

for coin in array:
    count += n//coin # 해당 화폐로 거슬러 줄 수 있는 동전의 개수 세기
    n %= coin # n = n % coin
print(count)
```

## 거스름 돈: 시간 복잡도 분석

- 화폐의 종류가 K라고 할 때, 소스코드의 시간 복잡도는 O(K)입니다.
- 이 알고리즘의 시간 복잡도는 거슬러줘야 하는 금액과는 무관하며, 동전의 총 종류에만 영향을 받습니다.

# [문제1] 1이 될 때까지

## 문제 설명

- 어떠한 수 **N이 1이 될 때까지** 다음의 두 과정 중 하나를 반복적으로 선택하여 수행하려고 합니다. 단, 두 번째 연산은 N이 K로 나누어 떨어질 때만 선택할 수 있습니다.
    1. N에서 1을 뺍니다.
    2. N을 K로 나눕니다.
- 예를 들어 N이 17, K가 4라고 가정합시다. 이때 1번의 과정을 한 번 수행하면 N은 16이 됩니다. 이후에 2번의 과정을 두 번 수행하면 N은 1이 됩니다. 결과적으로 이 경우 전체 과정을 실행한 횟수는 3이 됩니다. 이는 N을 1로 만드는 최소 횟수입니다.
- N과 K가 주어질 때 N이 1이 될 때까지 1번 혹은 2번의 과정을 수행해야 하는 최소 횟수를 구하는 프로그램을 작성하세요.

## 문제 조건

수행 시간: 2초

입력 조건: 첫째 줄에 N(1 <= N <= 100,000)과 K(2 <= K <= 100,000)가 공백을 기준으로 하여 각각 자연수로 주어집니다.

출력 조건: 첫째 줄에 N이 1이 될 때까지 1번 혹은 2번의 과정을 수행해야 하는 횟수의 최솟값을 출력합니다.

입력 예시:

```python
25 5
```

출력 예시:

```python
2
```

- 주어진 N에 대하여 최대한 많이 나누기를 수행하면 됩니다.
- N의 값을 줄일 때 2 이상의 수로 나누는 작업을 1을 빼는 작업보다 전체 연산의 횟수를 훨씬 많이 줄일 수 있습니다.
- 예를 들어 N=25, K=3일 때는 다음과 같습니다.

![](https://lh5.googleusercontent.com/n-pco9zHQ4zLfANuxs01xFn-9rclqlLGOC1zuvcdKg2bAIR0kmmTQAnnHKgFBGuJi0gxkqK4SyybJouGd0nt9bmzQU_8VQKUBr0D7LCiOWGGcYXMk2drdEWO_q9L8XZE8sGxww24j80=s1600)

## 정당성 분석

- 가능하면 최대한 많이 나누는 작업이 최적의 해를 항상 보장할 수 있을까요?
- N이 아무리 큰 수여도, K로 계속 나눈다면 기하급수적으로 빠르게 줄일 수 있습니다.
- 다시 말해 K가 2 이상이기만 하면, K로 나누는 것이 1을 뺴는 것보다 항상 빠르게 N을 줄일 수 있습니다.
    - 또한 N은 항상 1에 도달하게 됩니다.(최적의 해 성립)

## 답안

```python
# N, K을 공백을 기준으로 구분하여 입력받기
n, k = map(int, input().split())

result = 0

while True:
    # N이 K로 나누어 떨어지는 수가 될 때까지 빼기
    target = (n//k) * k
    result += (n-target)
    n = target
    # N이 K보다 작을 때(더 이상 나눌 수 없을때)반복문 탈출
    if n < k:
        break
    # K로 나누기
    n //= k # n = n//k
    result += 1
# 마지막으로 남은 수에 대하여 1씩 빼기
result += (n-1)
print(result)
```

# [문제2] 곱하기 혹은 더하기

## 문제 설명

- 각 자리가 숫자(0부터 9)로만 이루어진 문자열 S가 주어졌으 때, 왼쪽부터 오른쪽으로 하나씩 모든 숫자를 확인하며 숫자 사이에 'x'혹은 '+' 연산자를 넣어 결과적으로 만들어질 수 있는 가장 큰 수를 구하는 프로그램을 작성하세요. 단, +보다 x를 먼저 계산하는 일반적인 방식과는 달리, 모든 연산은 왼쪽에서 부터 순서대로 이루어진다고 가정합니다.
- 예를 들어 02984라는 문자열로 만들 수 있는 가장 큰 수는 ((((0 + 2) x 9) x 8) x 4) = 576입니다. 또한 만들어질 수 있는 가장 큰 수는 항상 20억 이하의 정수가 되도록 입력이 주어집니다.(프로그래밍 언어에서 정수가 가질 수 있는 최대 범위가 약 -2147000000 ~ 2147000000까지 이므로)

## 문제 조건

수행 시간: 1초, 기출: Facebook 인터뷰문제

입력 조건: 첫째 줄에 여러 개의 숫자로 구성된 하나의 문자열 S가 주어집니다.(1<= S의 길이 <= 20)

출력 조건: 첫째 줄에 만들어질 수 있는 가장 큰 수를 출력합니다.

입력 예시1:

```python
02984
```

출력 예시1:

```python
576
```

입력 예시2:

```python
567
```

출력 예시2:

```python
210
```

## 문제 해결 아이디어

- 대부분의 경우 '+'보다는 'x'가 더 값을 크게만듭니다.
    - 예를 들어 5 + 6 = 11이고, 5 x 6 = 30입니다.
- 다만 두 수 중에서 하나라도 '0' 혹은 '1'인 경우, 곱하기 보다는 더하기를 수행하는 것이 효율적입니다.
- 따라서 두 수에 대하여 연산을 수행할 때, 두 수중에서 하나라도 1 이하인 경우에는 더하며, 두 수가 모두 2 이상인 경우에는 곱하면 정답입니다.

## 답안

```python
data = input()

#첫 번째 문자를 숫자로 변경하여 대입
result = int(data[0])

for i in range(1, len(data)):
    #두 수 중에서 하나라도 '0' 혹은 '1'인 경우, 곱하기 보다는 더하기 수행
    num = int(data[i])
    if num <= 1 or result <= 1:
        result += num
    else:
        result *= num
print(result)
```

# [문제3] 모험가 길드

## 문제 설명

- 한 마을에 모험가가 N명 있습니다. 모험가 길드에서는 N명의 모험가를 대상으로 '공포도'를 측정했는데, '공포도'가 높은 모험가는 쉽게 공포를 느껴 위험 상황에서 제대로 대처할 능력이 떨어집니다.
    
- 모헌가 길드장인 길동이는 모험가 그룹을 안전하게 구성하고자 공포도가 X인 모험가는 반드시 X명 이상으로 구성한 모험가 그룹에 참여해야 여행을 떠날 수 있도록 규정했습니다.
    
- 길동이는 최대 몇 개의 모험가 그룹을 만들 수 있는지 궁금합니다. N명의 모험가에 대한 정보가 주어졌을 때, 여행을 떠날 수 있는 그룹의 수의 최댓값을 구하는 프로그램을 작성하세요.
    
- 예를 들어 N=5이고, 각 모험가의 공포도가 다음과 같다고 가정합시다.
    

```python
2 3 1 2 2
```

- 이 경우 그룹 1에 공포도가 1, 2, 3인 모험가를 한 명씩 넣고, 그룹 2에 공포도가 2인 남은 두명을 넣게 되면 총 2개의 그룹을 만들 수 있습니다.
- 또한 몇 명의 모험가는 마을에 그대로 남아 있어도 되기 때문에, 모든 모험가를 특정한 그룹에 넣을 필요는 없습니다.

## 문제 조건

수행 시간:*1초

입력 조건:

첫쨰 줄에 모험가의 수 N이 주어집니다. (1 <= N <= 100,000)

둘째 줄에 각 모험가의 공포도의 값을 N이하의 자연수로 주어지며, 각 자연수는 공백으로 구분합니다.

출력 조건:

여행을 떠날 수 있는 그룹 수의 최댓값을 출력합니다.

입력 예시:

```python
5
2 3 1 2 2
```

출력 예시:

```python
2
```

## 문제 해결 아이디어

- 오름차순 정렬 이후에 공포도가 가장 낮은 모험가 부터 하나씩 확인합니다.

![](https://lh6.googleusercontent.com/JHOeBNoUbvagIcCXmEr682Z5DqKWmh6RgtHahqJLDMktpNJLseYYhjSN6moqLzZeMVdukZldlQOUZqyu0f7tIuts_DbKzvzKbRKAKKIbOHqfAZTF2zhyrqh3f_ClubhOcZbRzv3oUL8=s1600)

- 앞에서부터 공포도를 하나씩 확인하며 '현재 그룹에 포함된 모험가의 수'가 '현재 확인하고 있는 공포도'보다 크거나 같다면 이를 그룹으로 설정하면 됩니다.

![](https://lh4.googleusercontent.com/VOKoPYvmTjZjDmgVZiqd_YgMHYogeVYeVF4HRwVrDox1JMWabH4qkBUnnsnhghY5_MmdRpf2a2NmCYBW-k3Xvb1NT3FsYV4jUbJVSIYgJxouwdNU4E5bajCXArn2HBapWU5Cvv_OsJ4=s1600)

- 이러한 방법을 이용하면 공포도가 오름차순으로 정렬되어 있다는 점에서, 항상 최소한의 모험가의 수만 포함하여 그룹을 결성하게 됩니다.

## 답안

```python
n = int(input())
data = list(map(int, input().split()))
data.sort()

result = 0 # 총 그룹의 수
count = 0 # 현재 그룹에 포함된 모험가의 수

for x in data: # 공포도를 낮은 것부터 하니씩 확인하며
    count += 1 # 현재 그룹에 해당 모험가를 포함시키기
    if count >= x: # 현재 그룹에 포함된 모험가의 수가 현재의 공포도 이상이라면, 그룹 결성o
        result += 1 # 총 그룹의 수 증가시키키
        count = 0 # 현재 그룹에 모함된 모헌가의 수 초기화
print(result) # 총 그룹의 수 출력
```

# 구현: 시뮬레이션과 완전 탐색

## 구현(Implementation)

- 구현이란, 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정입니다.

![](https://lh5.googleusercontent.com/COo6wzi3_GOv5p-bdg4QTHb5ALvknw1t6O8-GAi0PPHfuXcGRDLIlypRHRf-qyW3b_7rkvL3t8Lz5gI951eE1P9tx-T-KrrhrJKXZxIAKfeXPW7vpp_m0w6d5Y16QN0C5jGNO5jlGWE=s1600)

- 흔히 알고리즘 대회에서 구현 유형의 문제란 무엇을 의미할까요?
    - 풀이를 떠올리는 것은 쉽지만 소스코드로 옮기기 어려운 문제를 지칭합니다.
- 구현 유형의 예시는 다음과 같습니다.
    - 알고리즘은 간단한데 코드가 지나칠 만큼 길어지는 문제
    - 실수 연산을 다루고, 특정 소수점 자리까지 출력해야 하는 문제
    - 문자열을 특정한 기준에 따라서 끊어 처리해야 하는 문제
    - 적절한 라이브러리를 찾아서 사용해야 하는 문제
- 일반적으로 알고리즘 문제에서의 2차원 공간은 행렬(Matrix)의 의미로 사용됩니다.(파이썬에서 2차원 리스트에 해당한다.)

![](https://lh6.googleusercontent.com/WdRIH8_TiEdzpGrnxhbmTEBfDd8Uv29TtC5Vb0v1Q-PgIa6ygUsKPdWtKpYL7sG9jhSz4cQRLxU-_vyoeb7fs1GiVzcLM1awvWdrNdIfu3YAxrVowY-qhEj-YXFSANlr-CukEdx6GPo=s1600)

```python
for i in range(5): # 행
    for j in range(5): # 열
        print('(', i, ',', j, ')', end=' ')
    print()
```

- 시뮬레이션 및 완전 탐색 문제에서는 2차원 공간에서의 방향 벡터가 자주 활용됩니다.

![](https://lh5.googleusercontent.com/jUHsRqIBsZ4yY3XwEu8ZLYyvG_dn-qp5Dzl_e2AIJZB8m9Oes13eqiuudgCWwr6Ps-xKURHKBsZe_KsX6hIC2qhvu0HAFrFAh2GRIIpuwHpHUAUmmqGoUc4dB4CR71nAl9DNMOI9keA=s1600)

```python
# 동, 북, 서, 남
dx = [0, -1, 0, 1]
dy = [1, 0, -1, 0]

#현재 위치
x, y = 2, 2

for i in range(4):
    # 다음 위치
    nx = x + dx[i]
    ny = y + dy[i]
    print(nx, ny)
```

# [문제4(구현 중 시뮬레이션 유형)] 상하좌우

## 문제 설명

- 여행가 A는 N x N 크기의 정사각형 공간 위에 서 있습니다. 이 공간은 1 x 1크기의 정사각형으로 나누어져 있습니다. 가장 왼쪽 위 좌표는 (1, 1)이며, 가장 오른쪽 아래 좌표는 (N, N)에 해당합니다. 여행가 A는 상, 하, 좌, 우 방향으로 이동할 수 있으며, 시작 좌표는 항상 (1, 1)입니다. 우리 앞에는 여행가 A가 이동할 계획이 적힌 계획서가 놓여 있습니다.
    
- 계획서에는 하나의 줄에 띄어쓰기를 기준으로 하여 **L, R, U, D** 중 하나의 문자가 반복적으로 적혀 있습니다. 각 문자의 의미는 다음과 같습니다.
    
    - L: 왼쪽으로 한 칸 이동
    - R: 오른쪽으로 한 칸 이동
    - U: 위쪽으로 한 칸 이동
    - D: 아래쪽으로 한 칸 이동
- 이때 여행가 A가 N x N크기의 정사각형 공간을 벗어나는 움직임은 무시됩니다. 예를 들어 (1, 1)의 위치에서 L 혹은 U를 만나면 무시됩니다. 다음은 N = 5인 지도와 계획서입니다.
    
    ![](https://lh5.googleusercontent.com/9DVdfe2ViadvGieOjuXbpZ0DGbso6GwcwBPS-HczUYbUHNN8PwtXJs_EMXTs-zZQBcC7cIGkjlT_gFOmhjgI_2xHi9bqmcjlqSsTeLoGHvkL43a-j51_A7a1_-e_bAZXztwfAqrtFEg=s1600)
    

## 문제 조건

수행 시간: 2초

입력 조건:

- 첫째 줄에 공간의 크기를 나타내는 N이 주어집니다.(1 <= N <= 100)
- 둘째 줄에 여행가 A가 이동할 계획서 내용이 주어집니다.(1 <= 이동 횟수 <= 100)

출력 조건:

첫째 줄에 여행가 A가 최종적으로 도착할 지점의 좌표 (X, Y)를 공백을 기준으로 구분하여 출력합니다.

입력 예시:

```python
5
R R R U D D
```

출력 예시:

```python
3 4
```

## 문제 해결 아이디어

- 이 문제는 요구사항대로 충실히 구현하면 되는 문제입니다.
- 일련의 명령에 따라서 개체를 차례대로 이동시킨다는 점에서 시뮬레이션(Simulation) 유형으로도 분류되며 구현이 중요한 대표적인 문제 유형입니다.
    - 다만, 알고리즘 교재나 문제 풀이 사이트에 따라서 다르게 일컬을 수 있으므로, 코딩 테스트에서의 시뮬레이션 유형, 구현 유형, 완전 탐색 유형은 서로 유사한 점이 많다는 정도로만 기억합시다.

## 답안

```python
# N 입력 받기
n = int(input())
x, y = 1, 1
plans = input().split()

# L, R, U, D에 따른 이동 방향(방향 벡터 사용)
dx = [0, 0, -1, 1] # 행
dy = [-1, 1, 0, 0] # 열
move_types = ['L', 'R', 'U', 'D']
nx, ny = 0, 0 # 선언 및 초기화 과정 필수는 아님

# 이동 계획을 하나씩 확인하기
for plan in plans:
    # 이동 후 좌표 구하기
    for i in range(len(move_types)):
        if plan == move_types[i]:
            nx = x + dx[i]
            ny = y + dy[i]
    # 공간을 벗어나는 경우 무시
    if nx < 1 or ny < 1 or nx > n or ny > n:
        continue
    # 이동 수행
    x, y = nx, ny
 
print(x, y)
```

# [문제5] 시각

## 문제 설명

- 정수 N이 입력되면 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램을 작성하세요. 예를 들어 1을 입력했을 때 다음은 3이 하나라도 포함되어 있으므로 세어야 하는 시각입니다.
    - 00시 00분 03초
    - 00시 13분 30초
- 반면에 다음은 3이 하나도 포함되어 있지 않으므로 세면 안 되는 시각입니다.
    - 00시 02분 55초
    - 01시 27분 45초

## 문제 조건

수행 시간: 2초

입력 조건:

- 첫째 줄에 정수 N이 입력됩니다. (0 <= N <=23)

출력 조건:

- 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 출력합니다.

입력 예시

```python
5
```

출력 예시

```python
11475
```

## 문제 해결 아이디어

- 이 문제는 가능한 모든 시각의 경우를 하나씩 모두 세서 풀 수 있는 문제입니다.
- 하루는 86,400초이므로, 00시 00분 00초부터 23시 59분 59초까지의 모든 경우는 86,400가지 입니다.
    - 24 x 60 x 60 = 86,400
- 따라서 단순히 시각을 1씩 증가시키면서 3이 하나라도 포함되어 있는지를 확인하면 됩니다.
- 이러한 유형은 완전탐색(Brute Forcing) 문제 유형이라고 불립니다.
    - 가능한 경우의 수를 모두 검사해보는 탐색 방법(=구현문제)을 의미합니다.

# 답안

```python
# H 입력 받기
h = int(input())

count = 0
for i in range(h+1):
    for j in range(60):
        for k in range(60):
            # 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
           	if '3' in str(i) + str(j) + str(k):
                count += 1
print(count)
```

# [문제6] 왕실의 나이트

## 문제 설명

- 행복 왕국의 왕실 정원은 체스판과 같은 8 x 8 좌표 평면입니다. 왕실 정원의 특정한 한 칸에 나이트가 서있습니다. 나이트는 매우 충성스러운 신하로서 매일 무술을 연마합니다.
- 나이트는 말을 타고 있기 때문에 이동을 할 때는 L자 형태로만 이동할 수 있으며 정원 밖으로는 나갈 수 없습니다.
- 나이트는 특정 위치에서 다음과 같은 2가지 경우로 이동할 수 있습니다.
    1. 수평으로 두 칸 이동한 뒤에 수직으로 한 칸 이동하기
    2. 수직으로 두 칸 이동한 뒤에 수평으로 한 칸 이동하기

![](https://lh6.googleusercontent.com/34-zrTHVd_UJxPpsSCtJwoHQwZSAvsKnQNvRusqqOm2my03BhPlYwBqnTAWudQMx5Y68ZjxEK7Kn6xqSjvOLZR6Fz1icpuKEctPFrXOc2FtQjQ24RWOtdjYuev_U2ila-R6jMrGz9-M=s1600)

- 이처럼 8 x 8 좌표 평면상에서 나이트의 위치가 주어졌을 때 나이트가 이동할 수 있는 경우의 수를 출력하는 프로그램을 작성하세요. 왕실의 정원에서 행 위치를 표현할 때는 1부터 8로 표현하며, 연 위치를 표현할 때는 a부터 h로 표현합니다.
    - c2에 있을 때 이동할 수 있는 경우의 수는 6가지 입니다.

![](https://lh5.googleusercontent.com/VCUw0_t0rK67tl0tz3hilmT8cnGBI5Q9EFcs4URGeEwlF2DW3ICWrFqifacquy5isUsbyTV7W3vrilS2aQLPREohnV66lC6mlwcBdZZYphRQ3ngvtCS6g-LrpYX2NVenQzJhtfeBmI8=s1600)

- a1에 있을 때 이동할 수 있는 경우의 수는 2가지 입니다.

![](https://lh3.googleusercontent.com/ECvoFdeomxTn5Met5qyQI1Nlen1dFWrlaPQTEKVd2gGBeOXTIczkxhLSmxb7vjcI7k8gDZlHtDIxAWZRc3nUUjwfxrdQxp4Tt7aVAkaZt9yz9J9r3TjVrcnop0kZnbiSKL0DnJTyefY=s1600)

이 문제는 전형적인 시뮬레이션 완전탐색 문제 유형이면서도 2차원 좌표를 이용하는 구현문제로 볼 수 있습니다.

## 문제 조건

수행 시간: 1초

입력 조건:

- 첫째 줄에 8 x 8 좌표평면상에서 현재 나이트가 위치한 곳의 좌표를 나타내는 두 문자로 구성된 문자열이 입력된다. 입력문자는 a1처럼 열과 행으로 이루어진다.

출력 조건:

- 첫째 줄에 나이트가 이동할 수 있는 경우의 수를 출력하시오

입력 예시:

```python
a1
```

출력 예시:

```python
2
```

## 문제 해결 아이디어

- 이 문제는 구현문제로써 주어진 요구사항대로 충실히 구현하면 되는 문제입니다.
- 나이트의 8가지 경로를 하나씩 확인하며 각 위치로 이동이 가능한지 확인합니다.
    - 리스트를 이용하여 8가지 방향에 대한 벡터를 정의합니다.

# 답안

```python
# 현재 나이트의 위치 입력받기
input_data = input()
row = int(input_data[1])
column = int(ord(input_data[0])) - int(ord('a')) + 1

# 나이트가 이동할 수 있는 8가지 방향 정의(dx, dy 따로따로 하지 않고 한 리스트로 표현하는 방법)
steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, 2), (-2, 1)]

# 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
result = 0
for step in steps:
    # 이동하고자 하는 위치 확인
    next_row = row + step[0]
    next_colunm = column + step[1]
    # 해당 위치로 이동이 가능하다면 카운트 증가
    if next_row >= 1 and next_row <= 8 and next_column >= 1 and next_column <= 8:
        result += 1
        
print(result)
```

# [문제7] 문자열 재정렬

## 문제 설명

- 알파벳 대문자와 숫자(0~9)로만 구성된 문자열이 입력으로 주어집니다. 이때 모든 알파벳을 오름차순으로 정렬하여 이어서 출력한 뒤에, 그 뒤에 모든 숫자를 더한 값을 이어서 출력합니다.(수 != 숫자)
- 예를 들어 K1KA5CB7이라는 값이 들어오면 ABCKK13을 출력합니다.

## 문제 조건

수행 시간: 1초, 기출: Facebook 인터뷰

입력 조건:

- 첫째 줄에 하나의 문자열 S가 주어집니다.(1 <= S의 길이 <= 10,000)

출력 조건:

- 첫째 줄에 문제에서 요구하는 정답을 출력합니다

입력 예시1:

```python
K1KA5CB7
```

출력 예시1:

```python
ABCKK13
```

입력 예시2:

```python
AJKDLSI412K4JSJ9D
```

출력 예시2:

```python
ADDIJJJKKLSS20
```

## 문제 해결 아이디어

- 이 문제는 구현문제이므로 주어진 요구사항대로 충실히 구현하면 되는 문제입니다.
- 문자열이 입력되었을 때 문자를 하나씩 확인합니다.
    - 숫자인 경우 따로 합계를 계산합니다.
    - 알파벳의 경우 별도의 리스트에 저장합니다.
- 결과적으로 리스트에 저장된 알파벳을 정렬해 출력하고, 합계를 뒤에 붙여 출력하면 정답입니다.

## 답안

```python
data = input()
result = []
value = 0

# 문자를 하나씩 확인하며
for x in data:
    # 알파벳인 경우 결과 리스트에 삽입
    if x.isalpha():
        result.append(x)
    # 숫자는 따로 더하기
    else:
        value += int(x)
# 알파벳을 오름차순으로 정렬
result.sort()

# 숫자가 하나라도 존재하는 경우 가장 뒤에 삽입
if value != 0:
    result.append(str(value))
#최종 결과 출력(리스트를 문자열로 변환하여 출력)
print(''.join(result))
```