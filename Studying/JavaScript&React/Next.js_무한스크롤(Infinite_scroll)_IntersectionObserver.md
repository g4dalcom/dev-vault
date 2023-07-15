# 개요

![](https://blog.kakaocdn.net/dn/CnjX1/btsnGPBb1EL/lTdtegEtOanrVX67JbG4DK/img.gif)

- 무한스크롤을 구현하는 방법에는 스크롤의 높이와 클라이언트(화면)의 높이를 계산하는 스크롤 이벤트 기반 구현 방식도 있지만 ([참고 사이트1](https://ghur2002.medium.com/react%EC%97%90%EC%84%9C-infinite-scroll-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-128d64ea24b5),[참고 사이트 2](https://velog.io/@hyounglee/TIL-56))
- 저는 **IntersectionObserver** 라는 WebAPI를 이용해서 무한스크롤을 구현해보았습니다.
- 스크롤 이벤트 기반 방식은 스크롤이 될 때마다 자바스크립트 엔진이 계속해서 함수를 호출하게 되는데 
- **Intersection observer**는 브라우저단에서만 동작하고 필요할 때만 자바스크립트 코드를 불러오므로 성능상에서도 이점이 있고 
- 무한 스크롤 외에도 여러 곳에서 사용할 여지가 있는 확장성이 있는 API라는 생각이 들었기 때문입니다!


## 동작 방식

- **IntersectionObserver**의 동작 방식은 매우 직관적이고 심플합니다.
1. 관찰자(observer) 생성
2. 관찰 대상(target) 생성
3. 관찰 대상이 조건을 만족할 때 **콜백 함수** 실행

- 조건은 아래와 같은 3가지 경우입니다.
	- 관찰 대상(target)이 등록됐을 때
	- 대상이 화면에서 안보이다가 나타났을 때
	- 대상이 화면에서 보이다가 사라졌을 때

```typescript
// 1.
const observer = new IntersectionObserver(callback, options) => {}, options)

// 2.
observer.observe(target);
```

- 무한 스크롤을 구현할 때는 관찰 대상을 리스트 끝에 생성해두고 관찰 대상이 발견되었을 때(스크롤을 리스트 끝까지 내렸을 때) 새로운 데이터를 추가적으로 요청(callback)하면 되겠네요!

## 구현

### useIntersectionObserver.tsx

```typescript
const useIntersectionObserver = (callback: () => void) => {
  const [observeTarget, setObserveTarget] = useState(null);

  const observer = useRef<MutableRefObject<Element> | any>(null);

  useEffect(() => {
    observer.current = new IntersectionObserver(
      ([entry]) => {
        if (!entry.isIntersecting) return;
        callback();
      },
      { threshold: 1 },
    );
  });

```

- 먼저 observer를 생성하고 엔트리를 탐색합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgDj5Q%2FbtsnEWaeHuu%2F5KATBPoM0e0gG9UdK9pId0%2Fimg.png)

- 옵저버는 위와 같은 옵션을 갖습니다.

###### Options

- root : 기본값 null인 경우 브라우저의 뷰포트. 타겟의 가시성을 검사하기 위한 객체를 지정하는 값입니다
- rootMargin: root의 범위를 margin값을 이용해 확장하거나 축소할 수 있습니다.
- threshold: 옵저버가 탐지를 하기 위해 타겟의 가시성이 얼마나 필요한지를 나타낸 숫자입니다. 0인 경우 root가 타겟을 만나는 즉시 실행되고 1인 경우 타겟이 완전히 노출되었을 때 실행됩니다.

- entry는 **IntersectionObserverEntry 인스턴스 배열**인데 관찰하는 요소의 정보와 루트 요소의 정보가 들어있습니다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6koZi%2FbtsnEkCvT6U%2FRIPukgvYQKVrCmPNH4h3lK%2Fimg.png)

###### Options

- boundingClientRect: 관찰 대상의 사각형 정보
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcI58wg%2FbtsnG0Jtm6E%2FB6eXjIbIVFvHR8w4N6ScU1%2Fimg.jpg)

- intersectionRect: 관찰 대상의 교차한 영역 정보
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb1mVNA%2FbtsnF4k8szs%2FJYkumxYW7VI6Fc83ydTQOk%2Fimg.jpg)

- intersectionRatio: 관찰 대상의 교차한 영역 백분율(intersectionRect 영역에서 boundingClientRect 영역까지 비율)
- isIntersecting: 관찰 대상의 교차 상태
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2Sroa%2FbtsnFMkFDa1%2FFJIVRGWCzFgNGAX5OZBCb0%2Fimg.jpg)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjtncG%2FbtsnEneSNdA%2FdyZwOJ01dBIB805AhruF8K%2Fimg.jpg)

- rootBounds: 지정한 루트 요소의 사각형 정보)
- target: 관찰 대상 요소
- time: 변경이 발생한 시간 정보

#### useIntersectionObserver.tsx

```typescript
  useEffect(() => {
    const currentTarget = observeTarget;
    const currentObserver = observer.current;

    if (currentTarget) currentObserver.observe(currentTarget);

    return () => {
      if (currentTarget) {
        currentObserver.unobserve(currentTarget);
      }
    };
  }, [observeTarget]);

  return setObserveTarget;
```

- **observe**는 관찰 대상을 관찰하기 시작함을 의미하고 **unobserve**는 관찰을 중지함을 의미합니다.

#### InfiniteScroll.tsx

```typescript
const InfiniteScroll = () => {
  const url = 'https://developer-lostark.game.onstove.com/markets/items';

  const [fetchData, page, setPage] = useFetch(url, 'infinite');
  const [observe, setObserve] = useState(false);

  console.log(fetchData);

  const pageHandler = () => {
    setPage(page + 1);
  };

  useEffect(() => {
    pageHandler();
    setObserve(false);
  }, [observe]);

  const setObserveTarget: React.Dispatch<SetStateAction<any>> =
    useIntersectionObserver(() => {
      setObserve(true);
    });

  return (
    <section>
      <h2 className="text-3xl text-center text-orange-700 my-8">
        infinite scroll page
      </h2>
      <DataList data={fetchData} usage={'infinite'} />
      <div
        ref={setObserveTarget}
        style={{ backgroundColor: 'blue', padding: '10px' }}
      />
    </section>
  );
};
```
- 맨 아래에 div태그로 타겟을 지정해놓았습니다. 스타일을 준 것은 눈으로 타겟을 확인해보기 위해서입니다.
- 이제 데이터 리스트 아래에 놓인 타겟이 탐지되면 page+1을 한 후 그 값을 body에 넣어서 추가적인 API 요청을 하게 됩니다.

#### useFetch

```typescript
type FetchReturnType = [
  data: ItemsProps[],
  page: number,
  setPage: React.Dispatch<React.SetStateAction<number>>,
];

const useFetch = (url: string, usage: string): FetchReturnType => {
  const [fetchData, setFetchData] = useState<ItemsProps[]>([]);
  const [page, setPage] = useState(1);
  const APIkey = process.env.NEXT_PUBLIC_LOSTARK_API_KEY;

  useEffect(() => {
    fetch(url, {
      method: 'POST',
      headers: {
        Authorization: `Bearer ${APIkey}`,
        'content-type': 'application/json;charset=UTF-8',
      },
      body: JSON.stringify({
        Sort: 'RECENT_PRICE',
        CategoryCode: 40000,
        CharacterClass: '',
        ItemTier: null,
        ItemGrade: '',
        ItemName: '',
        PageNo: page,
        SortCondition: 'ASC',
      }),
    })
      .then((res) => res.json())
      .then((data) =>
        usage === 'pagination'
          ? setFetchData(data?.Items)
          : setFetchData([...fetchData, ...data?.Items]),
      );
  }, [page]);

  return [fetchData, page, setPage];
};
```

- 페이지네이션은 페이지가 넘어갔을 때 한 페이지당 정해진 데이터만 보여주면 되지만
- 무한 스크롤은 데이터가 점점 쌓여서 스크롤이 생기는 형식이기 때문에 데이터를 누적시켜주어야 합니다.
- 따라서 **usage** 에 따라 데이터를 다르게 저장하도록 하였습니다.

---

### 참고 자료

- [IntersectionObserverAPI로 무한스크롤 구현](https://simian114.gitbook.io/blog/undefined/react/intersectionobserverapi)
- [[블로그만들기] Intersection Observer로 무한스크롤 구현하기(typescript)](https://velog.io/@ongddree/%EB%B8%94%EB%A1%9C%EA%B7%B8%EB%A7%8C%EB%93%A4%EA%B8%B0-Intersection-Observer%EB%A1%9C-%EB%AC%B4%ED%95%9C%EC%8A%A4%ED%81%AC%EB%A1%A4-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0typescript)
- [Intersection Observer - 요소의 가시성 관찰](https://heropy.blog/2019/10/27/intersection-observer/)
- [[React] Intersection Observer API를 이용하여 Infinite Scroll 구현하기](https://devkkiri.com/post/927ae150-7627-4e25-b29f-2d0293b82332)