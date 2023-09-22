## 설정 방법

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzLH4w%2FbtsvdsL3wHj%2FkYKrbl4deoQmKz6MNkx2ok%2Fimg.png)

- queryClient의 옵션으로 staleTime과 cacheTime을 각각 설정해줄 수 있습니다.
- 기본적으로 staleTime은 0, cacheTime은 5분으로 되어있습니다.

### cache

- React-query는 설정한 cacheTime만큼 데이터를 메모리에 저장해놓습니다.
- 데이터를 fetching 하려고 할 때 캐시된 데이터가 있다면 API 요청을 하지 않고 캐시된 데이터를 가져오게 되겠죠?!
- 그런데 주의할 점은 캐시된 데이터가 있다고 무조건 fetching을 하지 않는 것은 아닙니다.
- 후술하겠지만 데이터가 stale 상태라면 새로 fetch를 해서 캐시 데이터가 바뀌게 될 것입니다.

### stale

- stale 상태의 반대는 fresh입니다. 즉 데이터가 신선한지 그렇지 않은지를 나타내는 상태입니다.
- React-query 공식 문서에서도 stale와 fresh 라는 단어를 사용해서 상태를 구분하니 직역한 그대로 이해하면 되겠습니다.
- 만약 staleTime 을 1000 * 60 * 5 (5분)이라고 설정해두었다면 data fetching이 되고 5분 동안 fresh 상태였다가 stale 상태로 바뀌는 것입니다.


### stale과 cache가 구분되는 이유

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb73VvF%2Fbtsvdva3CD6%2F2QltYvtgoYxGIzhtteWuak%2Fimg.png)

- 처음에는 두 개념이 와닿지가 않았습니다. 캐시로만 관리해도 같은 목적을 달성할 수 있다고 보였거든요!
- 위에서 잠깐 언급했지만 refetch하는 조건은 캐시가 만료되고 stale 상태일 경우입니다. 만약 stale이라는 개념이 없다면 캐시가 만료되었을 때 새로운 페이지로 이동한다면 데이터를 가져와서 그리기까지 Loading화면이 그려지겠죠.
- 그런데 stale 개념을 이용한다면 캐시 데이터가 남아있을 때 stale 상태여서 refetch를 하게 되므로 캐시된 데이터를 화면에 미리 보여주고 데이터를 새로 가져온 즉시 화면에 그려줄 수 있게 됩니다.
- 사용자는 Loading 화면보다 즉각적으로 바뀌는 화면을 보게 되겠지요!
- 이와 같은 이유로 staleTime보다 cacheTime이 항상 더 길어야 합니다.

---
### 참고 자료

- [캐싱에 대한 구현](https://velog.io/@chltjdrhd777/React-Query-%EC%BA%90%EC%8B%B1%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B5%AC%ED%98%84)
- [stale & cache 동작원리](https://www.timegambit.com/blog/digging/react-query/03#optionalremove)
