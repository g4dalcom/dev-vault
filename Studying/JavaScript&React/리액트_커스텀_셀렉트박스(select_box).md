# 개요

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHNnkN%2FbtsmA2Do0uJ%2FrQdKXEh1AbWEfmwZ8NncM0%2Fimg.png)

- 페이지 곳곳에서 사용되는 정렬 기능을 select box를 이용해 구현하고자 하였습니다.
- 피그마에서 보면 대충 위와 같은 select box입니다.
- 페이지마다 사용되는 정렬 내용이 다르기 때문에 재사용이 가능한 컴포넌트로 만들기 위해서는 정렬 옵션을 props로 받아야 할 것 같습니다.
- 그리고 옵션에 대한 value를 저장해야하므로 setValue 또한 받아야 할 것 같습니다.


## 요구사항

- select box를 누르면 선택 옵션이 아래로 펼쳐지고 선택을 하면 해당 옵션으로 value가 바뀌어야 합니다.
- 선택 옵션이 펼쳐졌을 때 외부를 클릭하면 현재 옵션 그대로 저장하면서 박스가 닫혀야 합니다.


# 구현

- 먼저 select box 외부를 클릭했을 때 옵션 리스트가 닫히도록 구현을 하려면 useRef를 이용해서 현재 선택된 element가 아닌 경우에 창을 닫도록 구현해야 합니다.
- 따라서 이 부분은 이전에 구현해놓은 useSelector 훅을 이용할 생각입니다.

#### useSelector

```typescript
import React, { useRef } from "react";
import { useEffect, useState } from "react";

type useSelect = [
  boolean,
  React.MutableRefObject<HTMLDivElement | null>,
  () => void
];

const useSelector = (): useSelect => {
  const [isOpen, setIsOpen] = useState<boolean>(false);
  const ref = useRef<HTMLDivElement | HTMLInputElement | null>(null);

  const toggleHandler = () => setIsOpen(!isOpen);

  const handleClickOutside = (e: React.BaseSyntheticEvent | MouseEvent) => {
    if (ref.current && !ref.current.contains(e.target)) setIsOpen(!isOpen);
  };

  useEffect(() => {
    if (isOpen) {
      window.addEventListener("click", handleClickOutside);
      return () => window.removeEventListener("click", handleClickOutside);
    }
  }, [isOpen]);

  return [isOpen, ref, toggleHandler];
};

export default useSelector;
```

- 드롭다운이나 select box 등 해당 요소가 선택되었는지(열렸는지) 여부를 확인하는 **state**와 해당 요소의 엘리먼트를 기억하는 **ref**, 그리고 해당 요소의 행동을 제어하는 **handler** 세 가지를 반환합니다.
- 외부를 클릭했을 때 선택을 해제하거나 창을 닫아야 하는 경우 등 매우 많은 경우에 사용되는 유용한 훅입니다!


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcaRkCo%2FbtsmBYtTLF4%2FyyG9dLAQ2nhFOw6fYI5KCK%2Fimg.png)

- **select** 태그와 **option** 태그는 기본적으로 모바일에서 해당 모바일 환경에 맞는 UI를 제공합니다.
- 따로 반응형으로 제작하지 않아도 편안한 UI가 노출된다는 장점이 있지만 커스텀하고자 할 때는 불편함이 있습니다.
- 그래서 select 태그와 option 태그 대신 **ul** 태그와 **li** 태그를 이용해 직접 커스텀하는 레퍼런스가 많았습니다.
- 저 역시 PC와 모바일에서 같은 UI를 보여주고 싶었고 반응형으로 제작해보는 경험도 해보고 싶었기 때문에 **ul** 태그와 **li** 태그를 이용해 커스텀 해보았습니다.


### 주요 UI

#### SelectBox.tsx

```typescript
      <S.SelectOptions isOpen={isSelected}>
        {options.map(option => (
          <S.Option key={option} value={option} onClick={handleSelectValue}>
            {option}
          </S.Option>
        ))}
      </S.SelectOptions>
```

#### style.ts

```css
export const SelectOptions = styled.ul<{ isOpen: boolean }>`
  position: absolute;
  top: 34px;
  left: 0;
  width: 100%;
  overflow: hidden;
  height: max-content;
  max-height: ${props => (props.isOpen ? "none" : "0")};
  padding: 0;
  border: ${props =>
    props.isOpen ? "2px solid var(--color-darkblue)" : "none"};
  border-radius: 8px;
  background: radial-gradient(
    190.97% 141.42% at 100% 100%,
    rgba(247, 247, 247, 0.7) 0%,
    rgba(247, 247, 247, 0.7) 100%
  );
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  color: #111;
  appearance: none;
`;
```

- 로직이 들어간 부분은 여기 한 곳입니다.
- option들을 담고 있는 전체 박스는 처음에 보이지 않다가 select box가 open된 경우(isSelected = true) 노출이 되게끔 만들었습니다.

### 주요 기능

#### SelectBox.tsx

```typescript
interface Props {
  options: string[];
  setOption: React.Dispatch<React.SetStateAction<string>>;
}

const SelectBox = ({ options, setOption }: Props) => {
  const [isSelected, selectRef, selectHandler] = useDetectClose();
  const [viewValue, setViewValue] = useState("정렬");

  const handleSelectValue = (e: any) => {
    setViewValue(e.target.getAttribute("value"));
  };

  return (
    <S.SelectBox ref={selectRef} onClick={selectHandler}>
      <S.Label>{viewValue}</S.Label>
      <S.SelectOptions isOpen={isSelected}>
        {options.map(option => (
          <S.Option key={option} value={option} onClick={handleSelectValue}>
            {option}
          </S.Option>
        ))}
      </S.SelectOptions>
      <SelectArrow />
    </S.SelectBox>
  );
};
```

- 맨 처음에 이야기했듯이 정렬 옵션들인 **options** 배열과 value를 설정하는 **setOption** 두 개를 props로 받게 됩니다.
- 그리고 option이 클릭될 때 li 태그의 value 속성의 값을 가져와서 저장하게 됩니다.

#### 고민거리

- 우선 options로 받는 문자열은 **\["최신순", "가격낮은순", "좋아요순"\]** 과 같은 형식으로 사용자에게 보여지는 문구 그대로입니다.
- 그러나 우리가 반환해야 하는 값은 **"newest", "priceasc", "mostlike"** 와 같은 API 요청시 필요한 options 값입니다.
- options로 반환할 값을 props로 입력받는다고 하여도 사용자에게 저대로 보여줄 수는 없으므로 어찌되었건 문자열을 변환해야만 했습니다.

```typescript
  const handleSelectValue = (e: any) => {
    const current = e.target.getAttribute("value");
    setViewValue(current);

    switch (current) {
      case "최신순":
        setOption("newest");
        break;
      case "오래된순":
        setOption("oldest");
        break;
      case "좋아요순":
        setOption("mostlike");
        break;
      case "조회수순":
        setOption("mostview");
        break;
      case "가격낮은순":
        setOption("priceasc");
        break;
      case "가격높은순":
        setOption("pricedesc");
        break;
    }
  };
```

- 그래서 위와 같은 로직이 추가되었습니다...
- 옵션의 value 속성은 props로 받는 문자열이 되고
- 옵션이 선택되었을 때 해당 li 태그의 value 속성값을 가져와서 저장한 뒤 사용자에게 노출됩니다.
- 그리고 그 과정에서 문자열 변환 작업을 합니다.
- 이 로직의 문제점은 정확히 입력받아야 하는 문자열이 정해져있다는 것입니다. 조회수순이 아니라 조회수높은순 혹은 최신순 대신 최신으로 입력이 오면 정확한 값을 반환받을 수 없습니다.
- 또 한가지는 정렬 옵션이 추가될 때마다 case 또한 추가로 입력해주어야 한다는 것입니다.

### 리팩토링(2023-07-29)

```typescript
if (usage === "정렬") {
      switch (current) {
        case "최신순":
          setOption("newest");
          break;
        case "오래된순":
          setOption("oldest");
          break;
        case "좋아요순":
          setOption("mostlike");
          break;
        case "조회수순":
          setOption("mostview");
          break;
        case "가격낮은순":
          setOption("priceasc");
          break;
        case "가격높은순":
          setOption("pricedesc");
          break;
      }
    } else if (usage === "상태") {
      switch (current) {
        case "판매완료":
          setOption("productst");
          break;
        case "판매중":
          setOption("productsf");
          break;
        case "등록대기":
          setOption("productwait");
          break;
        case "등록거절":
          setOption("productdeny");
          break;
      }
    } else {
      setOption(current);
    }

```

- 기존에 **정렬**용으로만 사용하던 셀렉트 박스를 **상태**와 **카테고리**용으로도 사용하게 되면서 위 로직이 더 길어지고 가독성이 안 좋아지게 되었습니다.

```typescript
interface Props {
  usage: "정렬" | "카테고리" | "상태";
  options: string[];
  setOption: React.Dispatch<React.SetStateAction<string>>;
}
```

- 입력받는 props도 usage라는 옵션이 추가되었습니다.
- 사실 **정렬**과 **상태**는 굳이 용도를 구분할 필요가 없이 기존 switch문에 추가를 해주어도 상관은 없습니다.
- 이것은 공통되는 부분으로 묶어줄 수 있다는 이야기가 될 수 있죠!
- 따라서 **입력받는 문자열**과 **대체되어야 하는 문자열** 두 가지의 키를 갖는 객체로 관리할 수 있을 거라고 생각하였습니다.

```typescript
  const replaceValue = {
    정렬: [
      { view: "최신순", replace: "newest" },
      { view: "오래된순", replace: "oldest" },
      { view: "좋아요순", replace: "mostlike" },
      { view: "조회수순", replace: "mostview" },
      { view: "가격낮은순", replace: "priceasc" },
      { view: "가격높은순", replace: "pricedesc" },
    ],
    상태: [
      { view: "판매완료", replace: "productst" },
      { view: "판매중", replace: "productsf" },
      { view: "등록대기", replace: "productwait" },
      { view: "등록거절", replace: "productdeny" },
    ],
    카테고리: [],
  };
```

- 카테고리의 경우 빈 배열로 둔 이유는 사용할 때 정렬이나 상태처럼 문자열이 변환되어야 하는 경우와 구분하기 위함입니다.

```typescript
  const handleSelectValue = (e: any) => {
    const current = e.target.getAttribute("value");
    setViewValue(current);

    try {
      if (replaceValue[usage].length > 0) {
        const temp = replaceValue[usage].filter(
          option => option.view === current
        );
        setOption(temp[0].replace);
      } else setOption(current);
    } catch (e) {
      console.error("올바른 옵션인지 확인해주세요", e);
    }
  };
```

- switch문으로 지저분하게 있던 로직은 위와 같이 리팩토링할 수 있었습니다.
- 카테고리의 경우는 length === 0 이기 때문에 입력받은 문자열 그대로 setOption을 하게 됩니다.
- 그 외 경우에는 current값과 같은 view를 가지는 객체의 replace값을 setOption하게 되는 것이죠!

### 사용 방법

- 사용할 페이지에서 우선 **value를 반환받아서 저장할 state**와 **정렬 옵션 배열**을 만들어야 합니다.
- 페이지마다 정렬에 사용할 option을 입력해주면 되고 위에서 이야기했지만 옵션의 문자열은 정해진대로만 입력해야 합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5kMFU%2FbtsmF7jeAtX%2FxxpzaOVsxRxpqTM2JGH7J1%2Fimg.png)

- 사용할 페이지에서 SelectBox 컴포넌트를 import 한 뒤 props를 넘겨줍니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbeQTgH%2FbtsmA2wsvIR%2FXiAMbh49ImCKDeIoZokb6K%2Fimg.png)

## 결과물

![](https://blog.kakaocdn.net/dn/8d4XP/btsmB9oksui/9zVtGNlrA8ptFc7YO1CIQ1/img.gif)

- 모양도 이쁘장하게 잘 나왔고 화면 크기에 따라 적당하게 조절이 됩니다.
- 콘솔창을 보면 value 값이 변환되어서 전달되는 것을 알 수 있습니다!


## 전체 코드

### SelectBox

```typescript
import { useState } from "react";
import * as S from "./style";
import SelectArrow from "../../assets/icons/SelectArrow";
import useDetectClose from "../../hooks/useDetectClose";

interface Props {
  options: string[];
  setOption: React.Dispatch<React.SetStateAction<string>>;
}

const SelectBox = ({ options, setOption }: Props) => {
  const [isSelected, selectRef, selectHandler] = useDetectClose();
  const [viewValue, setViewValue] = useState("정렬");

  const handleSelectValue = (e: any) => {
    const current = e.target.getAttribute("value");
    setViewValue(current);

    switch (current) {
      case "최신순":
        setOption("newest");
        break;
      case "오래된순":
        setOption("oldest");
        break;
      case "좋아요순":
        setOption("mostlike");
        break;
      case "조회수순":
        setOption("mostview");
        break;
      case "가격낮은순":
        setOption("priceasc");
        break;
      case "가격높은순":
        setOption("pricedesc");
        break;
    }
  };

  return (
    <S.SelectBox ref={selectRef} onClick={selectHandler}>
      <S.Label>{viewValue}</S.Label>
      <S.SelectOptions isOpen={isSelected}>
        {options.map(option => (
          <S.Option key={option} value={option} onClick={handleSelectValue}>
            {option}
          </S.Option>
        ))}
      </S.SelectOptions>
      <SelectArrow />
    </S.SelectBox>
  );
};

export default SelectBox;
```


### style.ts

```css
import styled from "styled-components";

export const SelectBox = styled.div`
  position: relative;
  width: 100px;
  height: 24px;
  flex-shrink: 0;
  border: 3px solid var(--color-darkblue);
  border-radius: 40px;
  background: radial-gradient(
    190.97% 141.42% at 100% 100%,
    rgba(247, 247, 247, 0.7) 0%,
    rgba(247, 247, 247, 0.7) 100%
  );
  margin: 0 10px 12px 0;
  backdrop-filter: blur(5px);
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  display: flex;
  align-items: center;
  font-size: var(--font-size-12);
  font-weight: 700;
  left: 10px;
  top: 10px;

  & svg {
    position: absolute;
    right: 3px;

    @media (max-width: 767px) {
      right: 0;
      width: 16px;
      height: 16px;
    }
  }

  @media (max-width: 767px) {
    width: 80px;
    height: 20px;
    border: 2px solid var(--color-darkblue);
    font-weight: 500;
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    width: 90px;
    height: 22px;
    border: 2px solid var(--color-darkblue);
  }
`;

export const Label = styled.span`
  width: 90%;
  text-align: center;
`;

export const SelectOptions = styled.ul<{ isOpen: boolean }>`
  position: absolute;
  top: 34px;
  left: 0;
  width: 100%;
  overflow: hidden;
  height: max-content;
  max-height: ${props => (props.isOpen ? "none" : "0")};
  padding: 0;
  border: ${props =>
    props.isOpen ? "2px solid var(--color-darkblue)" : "none"};
  border-radius: 8px;
  background: radial-gradient(
    190.97% 141.42% at 100% 100%,
    rgba(247, 247, 247, 0.7) 0%,
    rgba(247, 247, 247, 0.7) 100%
  );
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  color: #111;
  appearance: none;
`;

export const Option = styled.li`
  text-align: center;
  padding: 6px 0;
  transition: background-color 0.2s ease-in;
  border-bottom: 1px solid var(--color-lightivory);

  &:hover {
    background-color: var(--color-darkgreen);
  }
`;

```