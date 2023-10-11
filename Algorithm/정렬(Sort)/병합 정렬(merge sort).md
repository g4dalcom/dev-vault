# 개념

- 병합 정렬은 **분할 정복**과 **재귀** 를 이용한 알고리즘입니다.
- 주어진 배열을 원소가 하나만 남을 때까지 계속해서 절반씩 쪼갠 후 다시 병합을 하며 정렬하는 방식입니다.
- 반복의 수가 절반씩 줄어들기 때문에 O(logN)의 시간이 필요하며 병합할 때 모든 값들을 비교해야 하므로 O(N)의 시간이 필요합니다. 따라서 총 시간 복잡도는 **O(NlogN)** 이 됩니다!

# 정렬 방법

- 크게 보면 **분할** 과 **병합**의 단계로 나눌 수 있으며 병합 단계에서 병합 결과를 담을 배열이 추가로 필요합니다.
- **\[6, 5, 3, 1, 8, 7, 2, 4\]** 라는 배열을 정렬해보겠습니다.

#### 분할

- 배열의 요소들을 절반씩 쪼개어줍니다. 요소가 하나 남을 때까지 분할합니다.
	- \[6, 5, 3, 1\] \[8, 7, 2, 4\]
	- \[6, 5\] \[3, 1\] \[8, 7\] \[2, 4\]
	- \[6\] \[5\] \[3\] \[1\] \[8\] \[7\] \[2\] \[4\]


#### 병합

- 배열을 두 개씩 합쳐줍니다. 합치는 과정에서 작은 숫자가 앞으로 오도록 합니다.
	- \[5, 6\] \[1, 3\] \[7, 8\] \[2, 4\]
	- \[1, 3, 5, 6\] \[2, 4, 7, 8\]
	- \[1, 2, 3, 4, 5, 6, 7, 8\]


# 구현

```java
public class MergeSorter {
    public static void mergeSort(int[] arr) {
        int[] temp = new int[arr.length];
	    sort(arr, temp, 0, arr.length-1);
    }

    private static void sort(int[] arr, int[] temp, int lo, int hi) {
        if (lo >= hi) return;
        
        int mid = (lo + hi) / 2;
        sort(arr, temp, lo, mid);
        sort(arr, temp, mid+1, hi);
        merge(arr, temp, lo, mid, hi)
    }

    private static void merge(int[] arr, int[] temp, int lo, int mid, int hi) {
        int l = lo;
        int r = mid+1;
        int index = lo;
        
        while (l <= mid && r <= hi) {
            if (arr[l] <= arr[r]) {
                temp[index++] = arr[l++];
            } else {
                temp[index++] = arr[r++];
            }
        }
        
        while (l <= mid) {
            temp[index++] = arr[l++];
        }
        
        while (r <= hi) {
            temp[index++] = arr[r++];
        }
        
        for (int i = lo; i <= hi; i++) {
            arr[i] = temp[i];
        }
    }
}
```