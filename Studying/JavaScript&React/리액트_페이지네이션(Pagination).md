# 개요

![](https://blog.kakaocdn.net/dn/yG6H5/btsmUvD0hIY/gNGTo0qljsDX3dlZ3FWhy1/img.gif)

- 페이지네이션의 구현을 도와주는 좋은 라이브러리도 많지만
- next.js와 친숙해질겸 페이지네이션의 동작 방식도 익힐겸 간단하게 next.js 환경에서 구현해보았습니다.

### 요구 사항

- 모든 데이터를 한 번에 가져오는 것이 아니라 매 페이지를 누를 때마다 해당 페이지에 해당하는 API 요청만 합니다.
- 페이지의 총 개수는 전체 데이터 개수에 따릅니다. 예를 들어 한 페이지에 10개의 데이터를 표시하고 총 데이터의 개수가 38개라면 페이지의 개수는 4개가 되어야 합니다.
- **<** or **>** 기호를 누르면 페이지를 한 칸씩 이동합니다.


## 주요 로직

- 우선 화면에 보여줄 데이터를 불러와보겠습니다.

#### pagination/page.tsx

```typescript
export interface ItemsProps {
  Id: number;
  Name: string;
  Grade: string;
  Icon: string;
  BundleCount: number;
  TradeRemainCount: number;
  YDayAvgPrice: number;
  RecentPrice: number;
  CurrentMinPrice: number;
}

export interface FetchData {
  PageNo: number;
  PageSize: number;
  TotalCount: number;
  Items: ItemsProps[];
}

const [fetchData, setFetchData] = useState<ItemsProps[]>([]);
const [page, setPage] = useState(1);

const total = 50;
const limit = 10;
  
useEffect(() => {
fetch(`https://developer-lostark.game.onstove.com/markets/items`, {
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
  .then((data) => {
	setFetchData(data.Items);
  });
}, [page]);
```

- 가져올 데이터는 로스트아크의 거래소에 올라와있는 각인서 리스트입니다.
- 로스트아크 API key를 발급받은 후 API docs를 보고 body에 들어갈 데이터와 타입을 세팅해주었습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcrQrY9%2Fbtsm2HKoUxW%2Fhar96BaFN82m8RmLhKai41%2Fimg.png)

- 데이터는 위와 같이 응답이 오는데, PageNo은 1부터 10의 Size를 가지고 Items 배열 안에 리스트가 담겨서 옵니다.
- 그러면 한 페이지에 10개씩 보여주고(limit) 사용자가 페이지네이션의 숫자를 클릭한 경우 body의 pageNo에 해당 페이지네이션 숫자를 넣어서 요청을 보내면 될 것 같네요!
- 그리고 저는 데이터를 어떻게 나누어서 표시할 것인지 고민해보았습니다. 
- 만약 47개의 데이터가 있고 한 페이지에 10개의 데이터를 표시한다면 47 / 10 = 4.7 여기서 올림하면 결국 5개의 페이지가 필요하게 됩니다. 1~4 페이지는 10개의 데이터가 표시되고 5 페이지는 7개만 표시가 되겠죠!
- 위 계산을 하려면 우선 **총 데이터의 개수(total)** 와 **한 페이지에 표시할 데이터의 개수(limit)** 를 알아야 하겠네요!
- total의 경우 응답 데이터의 TotalCount를 이용해도 되지만 저는 임의로 50으로 정하고 진행해보았습니다.

#### component/DataList.tsx

```typescript
import { ItemsProps } from '../pagination/page';
import Image from 'next/image';

interface Props {
  data: ItemsProps[];
}

const DataList = ({ data }: Props) => {
  return (
    <>
      {data.map((e) => (
        <>
          <div key={e.Id} className="flex flex-col justify-center items-center">
            <Image src={e.Icon} width={48} height={48} alt="item image"></Image>
            <div className="text-l text-center  ">{e.Name}</div>
          </div>
        </>
      ))}
    </>
  );
};

export default DataList;
```

- 이 컴포넌트는 페이지에 데이터를 뿌려주는 역할을 합니다.
- 데이터가 표시되는 곳 아래에 페이지네이션을 위치시키면 적당할 것 같습니다.

#### pagination/page.tsx

```typescript
  return (
    <section>
      <h2 className="text-3xl text-center text-orange-700 my-8">
        This is pagination page
      </h2>
      <div className="text-xl w-screen text-center text-blue-600">
        LostArk Market!
      </div>
      <DataList data={fetchData} />
      <PaginationComponent
        total={total}
        page={page}
        limit={limit}
        setPage={setPage}
      />
    </section>
  );
};
```

- DataList에는 props로 데이터를 넘겨줍니다.
- 저의 경우는 데이터가 이미 10개씩 잘려서 오기 때문에 그대로 넘겨주면 되지만 
- **100개의 데이터를 미리 가져온 후 10개씩 잘라야 하는 경우**에는 **slice**를 이용해 데이터를 잘라주어야 합니다. 
- 또한 데이터를 잘라서 보낸다면, 어디서부터 어디까지를 잘라야 하는지를 알아야 합니다. 예를 들어서 1~100까지 10등분을 하는데 사용자가 3번 페이지네이션을 눌렀다면 30~39번에 해당하는 데이터를 잘라서 보내주어야 하는 것이죠!
- 이 때는 `const offset = (page - 1) * limit;` 의 수식을 이용해서 보낼 수 있습니다.
	- 1번 페이지의 시작 인덱스 : (1-1) * 10 = 0
	- 1번 페이지의 끝 인덱스 : 시작 인덱스 + limit-1
	- 1번 페이지의 범위 = 0 ~ 9
	- 2번 페이지의 시작 인덱스 : (2-1) * 10 = 10
	- 2번 페이지의 끝 인덱스 : 시작 인덱스 + limit-1
	- 2번 페이지의 범위 = 10 ~ 19
- 위와 같이 우리가 이미 알고 있는 정보들을 가지고 페이지의 범위를 알아낼 수 있습니다.
- props로 데이터를 넘겨준다면 `data={fetchData.slice(offset, offset + limit)}` 로 넘겨줄 수 있겠습니다.
- 그리고 페이지네이션 컴포넌트에는 **총 페이지 개수(total)**, **한 페이지에 나타낼 데이터 개수(limit)**, 그리고 현재 페이지를 알기 위한 **현재 페이지(page)** 와 페이지를 변화시킬 **setPage 함수** 를 prop로 넘기게 됩니다.


#### paginationComponent.tsx

```typescript
import { Dispatch } from 'react';

interface PaginationProps {
  total: number;
  page: number;
  limit: number;
  setPage: Dispatch<React.SetStateAction<number>>;
}

const PaginationComponent = ({
  total,
  page,
  limit,
  setPage,
}: PaginationProps) => {
  const pageNum = Math.ceil(total / limit);

  return (
    <section className="mt-8">
      <div className="flex justify-center gap-x-4">
        <button
          onClick={() => {
            setPage(page - 1);
          }}
          disabled={page === 1}
        >
          &lt;
        </button>
        {Array(pageNum)
          .fill(0)
          .map((_, i) => (
            <button
              key={i + 1}
              onClick={() => setPage(i + 1)}
              aria-current={page === i + 1 && 'page'}
            >
              {i + 1}
            </button>
          ))}
        <button
          onClick={() => {
            setPage(page + 1);
          }}
          disabled={page === pageNum}
        >
          &gt;
        </button>
      </div>
    </section>
  );
};

export default PaginationComponent;
```

- 앞서 이야기했듯이 **총 페이지의 개수(pageNum)**는 **총 데이터의 개수(total) / 한 페이지에 표시할 데이터 개수(limit)** 을 올림한 값이 됩니다.
- 그리고 총 페이지의 개수만큼 루프를 돌며 페이지 넘버버튼을 생성하게 됩니다.

```typescript
{Array(pageNum)
  .fill(0)
  .map((_, i) => (
	<button
	  key={i + 1}
	  onClick={() => setPage(i + 1)}
	  aria-current={page === i + 1 && 'page'}
>
	  {i + 1}
	</button>
  ))}
```

- 각 버튼은 눌렀을 때 자신의 번호를 setPage하게 되고 page의 변화를 감지하면 새로운 page를 요청 body의 pageNo에 실어서 POST 요청을 보낼 것 같네요!
- 그리고 **aria-current** 는 컴포넌트 내에서 현재 항목을 나타내는 키워드입니다. **page, step, location, date, time, true, false** 의 속성값을 가질 수 있는데 **aria-current = "page"** 라고 한다면 컴포넌트 내에서 현재 page라는 것을 명시해주는 것이죠!
- 그래서 onClick 되었을 때 **aria-current = page** 를 붙여주고 해당 키워드가 붙었을 경우를 조건으로 CSS를 입혀준다면 사용자 입장에서는 어떤 페이지에 머무르고 있는지를 확인할 수 있으며 스크린 리더를 사용하는 유저에게도 도움이 될 수 있습니다.

#### global.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  button[aria-current='page'] {
    @apply bg-yellow-300 font-bold rounded-3xl;
  }
}
```

- 또한 이전 페이지 버튼('<') 다음 페이지 버튼('>')은 각각 가장 처음 페이지와 마지막 페이지에서는 활성화가 되지 않도록 **disabled** 키워드를 조건으로 걸어주면 좀 더 사용자 UX에 좋은 페이지네이션이 될 것 같습니다!

## 전체 코드

#### pagination/page.tsx

```typescript
'use client';

import { useEffect, useState } from 'react';
import DataList from '../components/DataList';
import PaginationComponent from '../components/PaginationComponent';

export interface ItemsProps {
  Id: number;
  Name: string;
  Grade: string;
  Icon: string;
  BundleCount: number;
  TradeRemainCount: number;
  YDayAvgPrice: number;
  RecentPrice: number;
  CurrentMinPrice: number;
}

export interface FetchData {
  PageNo: number;
  PageSize: number;
  TotalCount: number;
  Items: ItemsProps[];
}

const Pagination = () => {
  /**
   * state: pokemon data list
   * page: current page
   * limit: The number of data to be displayed on one page
   * offset: Size between start and end points => fetchData.slice(offset, offset + limit)
   */
  const [fetchData, setFetchData] = useState<ItemsProps[]>([]);
  const [page, setPage] = useState(1);

  const total = 50;
  const limit = 10;
  const offset = (page - 1) * limit;
  const APIkey = process.env.NEXT_PUBLIC_LOSTARK_API_KEY;

  useEffect(() => {
    fetch(`https://developer-lostark.game.onstove.com/markets/items`, {
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
      .then((data) => {
        setFetchData(data.Items);
      });
  }, [page]);

  return (
    <section>
      <h2 className="text-3xl text-center text-orange-700 my-8">
        This is pagination page
      </h2>
      <div className="text-xl w-screen text-center text-blue-600">
        LostArk Market!
      </div>
      <DataList data={fetchData} />
      <PaginationComponent
        total={total}
        page={page}
        limit={limit}
        setPage={setPage}
      />
    </section>
  );
};

export default Pagination;
```

#### DataList.tsx

```typescript
import { ItemsProps } from '../pagination/page';
import Image from 'next/image';

interface Props {
  data: ItemsProps[];
}

const DataList = ({ data }: Props) => {
  return (
    <>
      {data.map((e) => (
        <>
          <div key={e.Id} className="flex flex-col justify-center items-center">
            <Image src={e.Icon} width={48} height={48} alt="item image"></Image>
            <div className="text-l text-center  ">{e.Name}</div>
          </div>
        </>
      ))}
    </>
  );
};

export default DataList;
```

#### PaginationComponent

```typescript
import { Dispatch } from 'react';

interface PaginationProps {
  total: number;
  page: number;
  limit: number;
  setPage: Dispatch<React.SetStateAction<number>>;
}

const PaginationComponent = ({
  total,
  page,
  limit,
  setPage,
}: PaginationProps) => {
  const pageNum = Math.ceil(total / limit);

  return (
    <section className="mt-8">
      <div className="flex justify-center gap-x-4">
        <button
          onClick={() => {
            setPage(page - 1);
          }}
          disabled={page === 1}
        >
          &lt;
        </button>
        {Array(pageNum)
          .fill(0)
          .map((_, i) => (
            <button
              key={i + 1}
              onClick={() => setPage(i + 1)}
              aria-current={page === i + 1 && 'page'}
            >
              {i + 1}
            </button>
          ))}
        <button
          onClick={() => {
            setPage(page + 1);
          }}
          disabled={page === pageNum}
        >
          &gt;
        </button>
      </div>
    </section>
  );
};

export default PaginationComponent;

```

#### global.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  button[aria-current='page'] {
    @apply bg-yellow-300 font-bold rounded-3xl;
  }
}
```

---

### 참고 자료

- [React로 페이지네이션 UI 구현하기](https://www.daleseo.com/react-pagination/)
- [\[React\] Pagination 구현하기](https://velog.io/@ahsy92/React-Pagination-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [MDN aria-current](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-current)
- [tailwind css로 aria-current 속성 활성화하기](https://velog.io/@tchaikovsky/tailwind.css-pagination-aria-current-%EC%86%8D%EC%84%B1%EC%9C%BC%EB%A1%9C-%ED%98%84%EC%9E%AC-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%B2%88%ED%98%B8-%ED%99%9C%EC%84%B1%ED%99%94%ED%95%98%EA%B8%B0)
