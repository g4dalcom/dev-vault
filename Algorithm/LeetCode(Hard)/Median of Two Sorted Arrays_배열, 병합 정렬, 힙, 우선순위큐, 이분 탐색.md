# [문제링크](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

## 📝 문제

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

**Input:** nums1 = [1,3], nums2 = [2]
**Output:** 2.00000
**Explanation:** merged array = [1,2,3] and median is 2.

**Example 2:**

**Input:** nums1 = [1,2], nums2 = [3,4]
**Output:** 2.50000
**Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`

---

### 💡 풀이

- 이 문제는 A라는 배열과 B라는 배열 두 개가 있을 때 둘을 합친 배열의 중간값을 찾는 문제입니다.
- 문제 요구사항에는 O(log(m+n)) 의 시간복잡도로 풀이를 하라고 되어있는데요. 요구 조건에서 input의 크기가 0 <= m, n <= 1,000 으로 크지가 않아서 O(m+n)의 시간복잡도 풀이로 풀어도 통과가 됩니다.
- 이분 탐색을 이용한 O(log(m+n))인 풀이는 여러 자료들을 통해 공부하였는데 릿코드 문제다보니 한글 자료도 부족하고 자세한 흐름을 설명한 글이 없어서 [유튜브](https://www.youtube.com/live/5sIrz4d1-EM?si=Q0NxLPAxsgiGDG1w) 와 [여러 블로그](## [Median of Two Sorted Arrays](https://engkimbs.tistory.com/entry/LeetCode-%EB%A6%AC%ED%8A%B8%EC%BD%94%EB%93%9C-Median-of-Two-Sorted-Arrays)) 들을 보고 참고하여 나름대로 정리해보았습니다.

### 🔍 정답 (병합 정렬, O(n + m))

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] nums = new int[nums1.length + nums2.length];
        int p1 = 0;
        int p2 = 0;
        int index = 0;

        while (p1 < nums1.length && p2 < nums2.length) {
            if (nums1[p1] == nums2[p2]) {
                nums[index++] = nums1[p1++];
                nums[index++] = nums2[p2++];
            } else if (nums1[p1] > nums2[p2]) {
                nums[index++] = nums2[p2++];
            } else {
                nums[index++] = nums1[p1++];
            }
        }

        while (p1 < nums1.length) nums[index++] = nums1[p1++];
        while (p2 < nums2.length) nums[index++] = nums2[p2++];

        if (nums.length % 2 == 0) {
            return (double) (nums[nums.length / 2] + nums[nums.length / 2 - 1]) / 2;
        } else {
            return (double) nums[nums.length / 2];
        }
    }
}
```

- 저 같은 경우는 먼저 병합 정렬을 하듯이 두 배열에 각각 0번 인덱스부터 포인터를 두고 더 작은 값을 새로운 배열에 넣는 식으로 구한 뒤, 홀수와 짝수일 경우의 중간값을 분기로 처리하였습니다.
- 그리고 다른 풀이 중에는 힙(우선순위 큐)을 이용한 풀이도 있었는데요.


### 🔍 정답(힙, 우선순위 큐)

```java
import java.util.Comparator;
import java.util.PriorityQueue;

class Solution {

    PriorityQueue<Integer> min = new PriorityQueue<>();
    PriorityQueue<Integer> max = new PriorityQueue<>(Comparator.reverseOrder());

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        enqueue(nums1);
        enqueue(nums2);

        if (min.size() > max.size()) {
            return (double) min.peek();
        }

        return (((double) min.peek()) + ((double) max.peek())) / 2f;
    }

    private void enqueue(int[] nums) {
        for (int num : nums) {
            if (min.size() <= max.size()) {
                min.add(num);
            } else {
                max.add(num);
            }

            if (min.size() == 0 || max.size() == 0) {
                continue;
            }

            if (min.peek() <= max.peek()) {
                int a = min.poll();
                int b = max.poll();

                max.add(a);
                min.add(b);
            }
        }
    }
}
```

- 임의의 중간값이 있을 때 중간값 왼쪽에는 중간값보다 작은 값이, 오른쪽에는 큰 값이 위치하겠죠?!
- 그러면 왼쪽 중 가장 큰 값과 오른쪽 중 가장 작은 값이 중간값과 맞닿아있는 값이라는 것을 이용한 풀이입니다.


- 그리고 이 세 번째 풀이가 문제의 의도와 맞게 시간복잡도 O(log(m+n)) 으로 구현된 풀이입니다.
- 이 문제가 hard인 이유가 있다고 생각되는 만큼 이해하기 어려웠던 풀이인데, 풀이가 복잡한 데에는 몇 가지 이유가 있습니다.
- 첫 번째로는 두 배열의 크기(m, n)가 같지 않다는 점, 두 번째로는 값의 중복이 있을 수 있다는 점 때문입니다.

### 🔍 정답(이분 탐색, O(log(m+n)))

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;

        if (m > n) {
            int[] numsTemp;
            numsTemp = nums1;
            nums1 = nums2;
            nums2 = numsTemp;

            int temp;
            temp = m;
            m = n;
            n = temp;
        }

        int left = 0;
        int right = m;

        while (left <= right) {
            int i = (left + right) / 2;
            int j = (m + n + 1) / 2 - i;

            if (i > left && nums1[i-1] > nums2[j]) {
                right = i - 1;
            } else if (i < right && nums2[j-1] > nums1[i]) {
                left = i + 1;
            } else {
                int maxLeft = 0;
                if (i == 0) maxLeft = nums2[j-1];
                else if (j == 0) maxLeft = nums1[i-1];
                else maxLeft = Math.max(nums1[i-1], nums2[j-1]);

                if ((m + n) % 2 == 1) return maxLeft;

                int minRight = 0;
                if (i == m) minRight = nums2[j];
                else if (j == n) minRight = nums1[i];
                else minRight = Math.min(nums1[i], nums2[j]);

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```

###### 기본 컨셉

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDr8ZA%2FbtsBj6RI2nS%2Fu2XQ75JOQD3tZ0sTBmtWk1%2Fimg.png)

- A, B 각 배열이 있을 때 가상의 C라는 배열의 길이(N)은 A.length + B.length 가 됩니다.
- 그리고 A와 B를 각각 반으로 나눈다고 했을 때, 작은 값 왼쪽들의 길이는 N/2이고 큰 값 오른쪽들의 길이는 N/2 가 되겠죠?!
- 그럼 아래와 같은 식도 성립이 됩니다. 왼쪽 값들(A_left + B_left)만 봤을 때 왼쪽 값들의 전체(N/2) 에서 하나를 빼면 다른 하나가 나오는 것은 당연한 것이니까요!
$$\frac{N}{2} - A\_left = B\_left$$
- 만약 A, B 각 배열의 크기가 10이라고 할 때, A_left가 6이라면 B_left는 자연스럽게 4가 되는 것처럼 하나를 기준으로 다른 하나의 값이 결정됩니다. **A_left 가 증가하면 B_left는 줄어들고 A_left가 감소하면 B_left는 증가하는** 성질을 이용해서 이분탐색을 할 수 있습니다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAJ9fK%2FbtsBqgsd47y%2FR36bZnDk5k8odovJ3PTkr0%2Fimg.png)

- 전체 배열을 절반으로 나누었을 때 왼쪽(작은 값들의 집합)에서 가장 큰 값과 오른쪽(큰 값들의 집합)에서 가장 작은 값이 중앙값이 됩니다.
- 그러면 두 배열이 나누어졌을 때도 p, r 중에 하나, q, s 중에 하나가 왼쪽과 오른쪽의 경계에 있는 값이 되겠죠.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcAalxT%2FbtsBq12rzXV%2FR7fNw66rfFcygA4VxZEUB0%2Fimg.png)

- 위에서 정의한 A, B 배열을 이용해 C 배열을 만들어보았습니다.
- 배열을 반으로 나누었을 때 왼쪽에는 모두 오른쪽보다 작은 값이 들어있어야 합니다.
- 우선 각각의 배열들은 이미 정렬이 되어있는 상태로 주어지므로 왼쪽에서 가장 큰 값은 Math.max(p, r) 일 것이고 오른쪽에서 가장 작은 값은  Math.min(q, s) 일 것입니다.
- 그런데 비교했을 때 대소 관계가 일치하지 않는 것을 볼 수 있습니다. 이 때 위에서 언급했던 성질을 이용할 수 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbed0vd%2FbtsBiUdesdT%2F5qKOQPNJfCmEhvnQ232im1%2Fimg.png)

- A_left의 길이를 줄이면 B_left의 길이가 늘어납니다. 그러면 자연스럽게 p와 r도 위치가 변경이 되죠!
- 그리고 C 배열도 다시 정의됩니다.
- 다시 Math.max(p, r) 를 한 값과 Math.min(q, s) 를 한 값을 비교하면 대소 관계가 일치함과 동시에 중앙값을 구할 수 있음을 알 수 있습니다.

#### 구현 과정

###### 배열의 길이가 다른 경우

- 한 가지 고려해야 할 점이 더 있습니다. 주어지는 A와 B의 배열의 길이가 일치하지 않는다는 점인데요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgTqER%2FbtsBjBLiFnE%2FcX5ZRYqzgEG3qRkPbiNJQ1%2Fimg.png)

- 위와 같이 배열의 길이가 다른 경우에 i-1과 j, 그리고 j-1과 i를 비교하게 될텐데 대소 관계가 일치하지 않을 경우 길이 조정을 하게 되겠죠?!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc6OYSd%2FbtsBqfUyd4k%2FKPbMlFDWe5OKD6XGfeLKB0%2Fimg.png)

- 위와 같이 A_left의 길이를 줄인다면 B_left의 길이는 늘어납니다.
- 만약 한 번 더 A_left의 길이를 줄인다면 j는 인덱스의 범위를 넘어가게 되겠네요.
- 그러면 우리는 A를 기준으로 길이를 조정하는 만큼 A의 길이가 B보다 길지 않아야 한다는 것을 알 수 있습니다.

```java
int m = nums1.length;
int n = nums2.length;

if (m > n) {
	int[] numsTemp;
	numsTemp = nums1;
	nums1 = nums2;
	nums2 = numsTemp;

	int temp;
	temp = m;
	m = n;
	n = temp;
}
```


###### 이분 탐색의 조건

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbevLkH%2FbtsBqDOq43x%2Fd3gDzLAXhyxXdo4zK9fmO0%2Fimg.png)

- A, B 배열이 있다면 A_left를 기준으로 조정하기로 하였습니다.
- A배열의 길이 안에서 A_left가 길어지고 줄어들고 할 것입니다. 따라서 이분 탐색의 초기값은 A 배열의 길이(m)입니다.
- i = A 배열의 중간 인덱스로 시작을 합니다. j 는 전체 배열의 중간값에서 i를 뺀 값을 기준으로 시작합니다.
- m + n + 1에서 +1을 해주는 이유는, 길이가 4인 두 배열이 있다고 할 때 전체 배열 8에서 중간값은 3, 4번 인덱스에 존재할 것입니다. 3번 인덱스에는 Math.max(p, r) 이 들어가고 4번 인덱스에는 Math.min(q, s) 가 들어가겠죠. 만약 전체 배열이 9로 홀수라면 4번 인덱스 하나만이 중간값이 되는데 이 값을 left쪽에 위치하게 하기 위함입니다.

```java
int left = 0;
int right = m;

while (left <= right) {
	int i = (left + right) / 2;
	int j = (m + n + 1) / 2 - i;

	if (i > left && nums1[i-1] > nums2[j]) {
		right = i - 1;
	} else if (i < right && nums2[j-1] > nums1[i]) {
		left = i + 1;
	} 
					.
					.
					.
```

###### 길이 조정의 결과

- 길이 조정을 해가며 중간값 후보들(중간값 왼쪽 값들 중 가장 큰 값, 중간값 오른쪽 값들 중 가장 작은 값)을 추려내야 하는데요!
- 전체 배열 C에서 중간값 왼쪽에 있는 값들 중에 가장 큰 값은 Math.max(i-1, j-1)로 구할 수 있습니다.
- 반대의 경우는 Math.min(i, j)로 구할 수 있죠.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFwA3H%2FbtsBrNJLdm8%2F3DYWc0uX6mmdoxNUlI4ZUk%2Fimg.png)

- 그런데 위 그림에서처럼 A_left를 계속 줄여서 p가 사라진 경우는 어떻게 될까요?
- p와 r중 큰 값이 전체 배열 C에서 maxLeft가 되는데 p가 사라졌으니 r인 j-1이 자연스럽게 maxLeft가 됩니다.
- 반대의 경우도 마찬가지로 구할 수 있겠죠!

```java
int maxLeft = 0;
if (i == 0) maxLeft = nums2[j-1];
else if (j == 0) maxLeft = nums1[i-1];
else maxLeft = Math.max(nums1[i-1], nums2[j-1]);

if ((m + n) % 2 == 1) return maxLeft;

int minRight = 0;
if (i == m) minRight = nums2[j];
else if (j == n) minRight = nums1[i];
else minRight = Math.min(nums1[i], nums2[j]);

```

- 위에서 잠깐 언급했듯이 두 배열의 길이의 합이 홀수라면 중간값은 하나만 존재합니다. C = \[1, 2, 3, 4, 5\] 라면 중간값은 3 하나만 존재하는 거죠! **m + n + 1 / 2 - i** 계산을 통해 왼쪽에 위치하게 했으므로 maxLeft를 그대로 리턴하면 되고 만약 짝수라면 중간값 오른쪽 값들 중 가장 작은 값을 구해서 (maxLeft + minRight) / 2.0 을 해주면 되는 것이죠!