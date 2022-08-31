---
keyword : Java
class : Grammer, Algorithm
---


### Arrays로 배열 다루기

- ##### 배열의 비교와 출력 - equals(), toString()
- toString()
	- java.util.Arrays.toString() 메소드는 파라미터로 배열을 입력받아서,
	- 배열에 정의된 값들을 문자열 형태로 만들어서 리턴해 줍니다.
	- System.out.prinln(arr) > 메모리의 주소값이 리턴됨
		int[] arr = {0, 1, 2, 3, 4};
		int[][] arr2D = {{11, 12}, {21, 22}};
			Arrays.toString(arr); > [0, 1, 2, 3, 4]
			Arrays.deppToString(arr2D); > //  [.[11, 12], [21, 22].]
		Arrays.equals(배열1, 배열2);
		
- ##### 배열의 복사 - copyOf(), copuOfRange()
		int[] arr = {0, 1, 2, 3, 4}
		int[] arr2 = Arrays.copyOf(arr, arr.length); // arr2 = [0, 1, 2, 3, 4]
		int[] arr3 = Arrays.copyOf(arr, 3); // arr3 = [0 , 1, 2]
		int[] arr4 = Arrays.copuOfRange(arr, 2, 4); // arr4 = [2, 3]
		
- ##### 배열의 정렬 - sort()
		int[] arr = {3, 2, 0, 1, 4};
		Arrays.sort(arr);  > arr = [0, 1, 2, 3, 4]