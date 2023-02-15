# 개념

- HTML 안의 특정 요소를 선택할 수 있는 도구


# 종류

### 전체 선택자
- HTML 페이지 내부의 모든 태그를 선택

```CSS
* { 
	box-sizing: border-box;
	margin: 0;
	padding: 0;
	}
```

### 태그 선택자
- 같은 태그명을 갖는 모든 요소를 선택
```css
a {
	text-decoration: none;
}
```

### 클래스 선택자
```css
.class1 {
	color: red;
}

div.class2 {  // 띄어쓰기 없음. 띄어쓰기 하면 하위 선택자가 됨
	color: yellow;
}
```

```html
<div class="class1">빨간색</div>

// div.class2는 div이면서 class2인 것만 변화시킨다.
<p class="class2">변화없다</p>

<div class="class2">노란색</div>
```


### 아이디 선택자
```css
#container {
	display: flex;
	justify-content: center;
	align-items: center;
}
```

##### id선택자와 class선택자의 용도
- 한 페이지 내에서 여러 번 반복될 필요가 있는 스타일은 `class 선택자`를 사용하고 단 한 번, 유일하게 적용될 스타일은 `id 선택자`를 사용한다.
- `class 선택자`는 글자색이 굵기 등 다른 곳에서도 적용할 수 있는 스타일일 때 사용하고 `id 선택자`는 웹 문서 안에서 요소의 배치와 같은 방법을 지정할 때 주로 사용한다.


### 복합 선택자

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcEsSD5%2FbtrZqTmHWfZ%2FQMMW1cvsftMzrnAayNTXv1%2Fimg.png)

#### 하위(후손) 선택자와 자식 선택자

```css
/* 하위(후손) 선택자 */
header p { border: 1px solid green; }

/* 자식 선택자 */
header > p { border: 1px solid blue; }
```


```html
/* 하위(후손) 선택자 */
<header>
	<p> <!-- 선택 -->
		<span>
			<p></p> <!-- 선택 -->
		</span>
	</p>
	<p> <!-- 선택 -->
		<span>
			<p></p> <!-- 선택 -->
		</span>
	</p>
</header>
```

```html
/* 자식 선택자 */
<header>
	<p> <!-- 선택 -->
		<span>
			<p></p>
		</span>
	</p>
	<p> <!-- 선택 -->
		<span>
			<p></p>
		</span>
	</p>
</header>
```

- 하위(후손) 선택자는 부모 요소에 포함된 모든 하위 요소에 적용
- 자식 선택자는 부모의 바로 아래 자식에게만 적용

#### 일반 형제 선택자와 인접 형제 선택자
- 같은 부모 요소를 가지는 요소들을 형제 관계라고 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLTtQ2%2FbtrZjKkKlLw%2FC05KIKQKmJlb4w9sIYFCX0%2Fimg.png)

```css
/* 일반 형제 선택자 */
p~ul { color: yellow }

/* 인접 형제 선택자 */
p+ul { color: red }
```

```html
/* 일반 형제 선택자
 * p~ul 일 때 p의 형제 요소 중 p 요소 뒤에 위치하는 모든  ul을 선택! **/

<p>The first paragraph.</p>
<ul>  <!-- 선택 -->
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ul>
<ul>  <!-- 선택 -->
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ul>
```

```html
/* 인접 형제 선택자
 * p+ul 일 때 p의 형제 요소 중 p 요소 바로 뒤에 위치하는 ul만 선택 **/

<p>The first paragraph.</p>
<ul>  <!-- 선택 -->
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ul>
<ul>
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ul>
```
- p와 ul 사이에 다른 요소가 존재하면 선택되지 않는다!

### 가상 클래스 선택자
- 요소의 특정 상태에서만 선택이 되는 가상의 클래스를 지정

```css
a:hover {
	color: red;
}

input:focus {
	background-color: yellow;
}
```

#### 동적 셀렉터
- `:link` 방문하지 않은 링크일 때
- `:visited` 방문한 링크일 때
- `:hover` 마우스가 올라와있을 때
- `:active` 클릭된 상태일 때
- `:focus`  포커싱 되었을 때

#### 구조 가상 셀렉터
- `:first-child` 셀렉터에 해당하는 모든 요소 중 첫번째 자식 요소를 선택
- `:last-child` 셀렉터에 해당하는 모든 요소 중 마지막 자식 요소를 선택
- `:nth-child(n)` 셀렉터에 해당하는 모든 요소 중 앞에서 n번째 자식 요소 선택
- `:nth-last-child(n)` 셀렉터에 해당하는 모든 요소 중 뒤에서 n번째 자식 요소 선택
- `:nth-child(odd/even)` (홀수/짝수)번째 자식인 요소 선택
- `:nth-child(2n)` 두번째 요소마다 선택(짝수)
- `:nth-child(2n+1)` 첫번째 요소부터 두번째 요소마다 선택(홀수)
- `:nth-child(n+5)` 다섯번째 요소부터 모두 선택
- `:nth-child(-n+5)` 앞에서부터 다섯개만 선택
- `:first-of-type` 대상 요소 형제 중 대상 요소가 첫번째로 확인되는 요소 선택
- `:last-of-type` 대상 요소 형제 중 대상 요소가 마지막으로 확인되는 요소 선택

###### :first-child 와 :first-of-type의 차이

```html
<div>
    <div>text1</div>
    <p>text2</p>
    <p>text3</p>
</div>
```

```css
/* Case1 */ 
div p:first-child { color: red; }

/* Case2 */ 
div p:first-of-type { color: red; }
```
- text2의 색을 변화시키고자 할 때 Case1의 경우는 변화가 없고 Case2의 경우에만 변화가 있다.
- `:first-child` 는 div 하위 요소 중 p 사이에 div 자식요소가 존재하기 때문에 선택이 되지 않는 것이고(요소들 중 첫번째 요소를 변화시키나 그 요소가 p여야 함)
- `:first-of-type` 는 p 요소만을 가지고 카운팅하기 때문에 선택할 수 있는 것이다.(p 요소들 중 첫번째를 선택한다는 의미)