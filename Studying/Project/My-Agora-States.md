# 개요

- 프로젝트명 : 아가리 스테이츠
- 프로젝트 소개 : HTML, CSS, JavaScript를 활용해서 질문 게시판 만들기
- 주요 기능
	- 질문 작성하기(모달창)
	- 로컬 스토리지에 데이터 저장 + 신규 데이터 저장
	- 페이지네이션
	- 이미지 애니메이션

[👉프로젝트 구경하기👈](https://g4dalcom.github.io/fe-sprint-my-agora-states/)

![](https://blog.kakaocdn.net/dn/Jx2dW/btr2ZB5jCVI/EbkJ05Q95eqzVanEY5me4K/img.gif)


# 상세

## 모달창으로 구현한 질문 작성하기

```javascript
// ======== 모달창 관련 ======== //
const $modal = document.querySelector('.modal');
const $form_container = document.querySelector('.form__container');
const $main_container = document.querySelector('#main_container');
const $bg = document.querySelector('.black_bg');

// 질문하기Btn 눌러서 모달 켜고 끄기
$modal.addEventListener('click', function () {
    if ($form_container.classList.contains('hide')) {
        $form_container.classList.remove('hide');
        $bg.style.display = 'block';
    } else {
        $form_container.classList.add('hide');
        $bg.style.display = 'none';
    }
});

// 모달창 바깥 클릭하면 끄기
window.addEventListener('click', (e) => {
    if (e.target === $bg) {
        $form_container.classList.add('hide');
        $bg.style.display = 'none';
    }
});
```
- 질문 작성하기 기능을 모달창으로 구현하였다.
- 모달창이 뜨면 뒷배경을 어둡게 하기 위해 `<div class='black_bg'</div>` 를 만들고 모달창이 떴을 때 display의 변화를 주었다.

```css
.black_bg {
    display: none;
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    top: 0;
    left: 0;
    z-index: 1;
}
```
- 전체 화면을 어둡게 만드는 클래스이다. 
- 기본적으로 display: none;으로 해서 보이지 않다가 모달창이 뜰 경우에 display: block;을 줘서 띄우는 방식
- 모달창의 z-index는 999이기 때문에 모달창은 덮이지 않는다.
- 그리고 덤으로, 모달창이 떴을 때 모달창 외 공간은 모두 .black_bg이므로 .black_bg가 클릭되었을 때 모달창이 사라지고 .black_bg 또한 display: none;으로 바꿔서 기본 화면이 뜰 수 있도록 함!


## 질문 작성하기

```javascript
const $submit = document.querySelector('.form');
$submit.addEventListener('submit', (e) => {
    const obj = {
        id: 'guest',
        createdAt: dateFormating(),
        title: title.value,
        author: author.value,
        url: '#',
        avatarUrl: 'https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdOTLKF%2Fbtr2OqwN8fe%2F75rJmFEs9jrg9n2Wx4X1X0%2Fimg.png',
        answer: null,
    };
    console.log(obj);

    let dataObj = JSON.parse(localStorage.getItem('agora'));
    dataObj.unshift(obj);
    console.log(dataObj);
    localStorage.setItem('agora', JSON.stringify(dataObj));
});
```
- form태그에서 submit했을 경우, 기본 이미지와 더불어 입력값(title, author)을 담은 객체를 저장해두고(obj)
- 기존에 로컬 스토리지에서 `agora`라는 키로 저장되어 있는 녀석들을 불러와서 맨 앞에 이번에 저장한 객체(obj)를 넣어준다(unshift)
- 그리고 새로 로컬 스토리지에 set 해주면 최신순으로 정렬되어 저장이 된다.

## 페이지네이션

### 전역변수 선언

```javascript
let pageNumber = 1;
const PAGE_SIZE = 9;
```
- 현재 pageNumber를 기억하기 위해 전역변수 선언
- 그리고 한 페이지에 보이는 요소의 개수는 9개로 하였다.


### 페이징 로직

```javascript
const getPagedDiscussions = (pageNumber) => {
    const pageSize = PAGE_SIZE;
    const startIndex = pageSize * (pageNumber - 1);
    const endIndex = startIndex + pageSize;

    return loadLocalStorageDate().slice(startIndex, endIndex);
};
```
- 기본 로직은, 로컬 스토리지에 있는 데이터 중에서 한 페이지에 그릴 만큼만 slice() 해서 가져오는 것이다. 그러기 위해서는 세 가지를 알아야 한다.
	- pageNumber
	- slice를 위한 index 2가지
- 1페이지라면 0번 인덱스부터 8번 인덱스까지(총 9개)의 데이터를 가져와야하고 그러려면 slice를 위한 시작 인덱스는 0, 마지막 인덱스는 9가 되어야 한다.(마지막 인덱스는 포함하지 않으므로)

### 페이징 바 관련

```javascript
const createNavi = (ul) => {
    const naviSize = Math.ceil(loadLocalStorageDate().length / PAGE_SIZE);
    const discussionFooter = document.querySelector('#pagination_container');

    // '<' 버튼을 누르면 pageNumber - 1 해주기
    const naviPrev = document.createElement('a');
    
    naviPrev.textContent = '<';
    naviPrev.className = 'pagination-button';
    naviPrev.href = '#';
    
    naviPrev.addEventListener('click', (e) => {
        let $active = document.getElementById(`${pageNumber}`);
        $active.classList.remove('active');
        e.preventDefault();
        pageNumber = pageNumber - 1;
        if (pageNumber < 1) pageNumber = 1;

        $active = document.getElementById(`${pageNumber}`);
        $active.classList.add('active');
        console.log('page', pageNumber);

        render(ul);
    });
    discussionFooter.appendChild(naviPrev);

    // '숫자' 버튼을 누르면 해당 pageNumber로 렌더링
    for (let i = 0; i < naviSize; i++) {
        const navi = document.createElement('a');
        
        navi.textContent = i + 1;
        navi.id = i + 1;
        navi.className = 'pagination-button';
        
        navi.addEventListener('click', (e) => {
            let $active = document.getElementById(`${pageNumber}`);
            $active.classList.remove('active');
            let num = e.target.textContent;
            pageNumber = Number(num);
            $active = document.getElementById(`${pageNumber}`);
            $active.classList.add('active');
            render(ul);
        });
        discussionFooter.appendChild(navi);
    }
    // 처음에 1번 페이지는 활성화 상태로!
    const init = document.getElementById('1');
    init.classList.add('active');

    // '>' 버튼을 누르면 pageNumber + 1 해주기
    const naviEnd = document.createElement('a');
    
    naviEnd.textContent = '>';
    naviEnd.className = 'pagination-button';
    naviEnd.href = '#';
    
    naviEnd.addEventListener('click', (e) => {
        let $active = document.getElementById(`${pageNumber}`);
        $active.classList.remove('active');
        e.preventDefault();
        pageNumber = pageNumber + 1;
        if (pageNumber > naviSize) pageNumber = naviSize;
        $active = document.getElementById(`${pageNumber}`);
        $active.classList.add('active');
        render(ul);
    });
    discussionFooter.appendChild(naviEnd);
};
```
- NaviSize는 페이징 바의 길이를 의미한다. 한 화면에 9개씩 그리는데 데이터가 10개일 경우 페이징 바는 2개가 되어야 하므로 `전체 데이터의 개수 / 한 화면에 그리는 데이터의 개수`를 올림해서 구한다!
- <, > 화살표를 이용한 이동과 페이지 넘버 자체를 눌러서 이동하는 경우 세가지를 나누어서 구현하였는데 switch 문으로 바꿔서 구현하면 중복코드가 좀 더 줄어들 것 같기도 하다...
- 그리고 `.active` 는 현재 위치를 표시하기 위한 클래스로 click 이벤트가 실행되면 현재 pageNumber의 .active 를 hide해주고 새롭게 구한 pageNumber에 .active 클래스를 add해서 표현하였다.


# 이미지 애니메이션

```css
/* ============ 메인화면 이미지 ============ */

figure {
    position: absolute;
    left: 50%;
    margin-left: -43px;
    top: 92px;
}

figure .motion-notebook {
    transform: rotate(25deg);
    position: relative;
    left: 500px;
    top: 200px;
    width: 100px;
    height: 100px;
    border-radius: 50%;
}

figure .motion-error {
    transform: rotate(-13deg);
    position: relative;
    width: 100px;
    height: 100px;
    border-radius: 50%;
    left: 170px;
}

figure .motion-esc {
    transform: rotate(25deg);
    position: relative;
    left: 100px;
    top: -500px;
    width: 100px;
    height: 200px;
}

figure .img_wrap .motion-cat {
    transform: rotate(-13deg);
    position: relative;
    left: 50px;
    top: -270px;
    width: 600px;
    height: 600px;
}

.img_wrap .motion-cat {
    top: 70px;
    z-index: 3;
}

.floating_effect {
    animation-name: floating-effect;
    animation-duration: 1.8s;
    animation-iteration-count: infinite;
}

.floating_effect1 {
    animation-name: floating-effect;
    animation-duration: 1.5s;
    animation-iteration-count: infinite;
}

.floating_effect2 {
    animation-name: floating-effect;
    animation-duration: 2.5s;
    animation-iteration-count: infinite;
}

.floating_effect3 {
    animation-name: floating-effect;
    animation-duration: 1.7s;
    animation-iteration-count: infinite;
}

@keyframes floating-effect {
    0% {
        transform: translateY(0%);
    }
    50% {
        transform: translateY(5%);
    }
    100% {
        transform: translateY(0%);
    }
}
```
- 이미지는 총 4개를 사용하였고 애니메이션 기능은 공통으로 Y축(위, 아래)을 기준으로 움직이게 된다.
- 그러나 모두 같은 움직임을 하면 생동감이 떨어지기 때문에 이미지마다 animation-duration을 다르게 해서 제각각 움직이는 것처럼 보이게 했다!