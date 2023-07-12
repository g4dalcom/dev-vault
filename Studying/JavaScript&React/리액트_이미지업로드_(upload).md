![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblwqRy%2Fbtsniteq42O%2Fw4rrmROvKcbhVu0wx4WQzK%2Fimg.png)

- 우선 피그마로 그려본 페이지 프로토타입 입니다.
- 해당 페이지에서는 사용자가 수거 신청을 할 의류의 이미지를 등록하고 물품의 내용을 적어서 보낼 수 있는데 상품 추가를 통해 한 번에 여러 개를 묶어서 보낼 수도 있습니다.

## 요구 사항

- 하나의 물품당 하나의 이미지만 등록이 가능합니다.
- 이미지가 등록이 되어있지 않거나 상세 내용 중 하나라도 빠진 곳이 있으면 에러가 나야합니다.
- 상품 추가 버튼을 누르면 새로운 등록 폼이 등장하고 추가로 입력해서 한 화면에서 여러 개의 물품을 한 번에 보낼 수 있어야 합니다.
- 입력하던 폼의 삭제 버튼을 누르면 해당 폼은 사라지고 요청에서 제외됩니다.
- 요청은 formData 형식으로 보내며 **productlist** 라는  key로 물품의 상세 내용 배열을 **문자열**로, 각 이미지 파일은 **files** 라는 key로 보냅니다.

## 구현

### 초기 작성 로직

#### CollectionPage.tsx

```typescript
export interface ContentsProps {
  name: string;
  content: string;
  category: string;
}

const CollectionPage = () => {
  const initialValue = {
    name: "",
    content: "",
    category: "",
  };

  const [formCount, setFormCount] = useState(1);
  const [images, setImages] = useState<File[]>([]);
  const [contents, setContents] = useState<ContentsProps[]>([initialValue]);
```

- 처음에 작성한 로직은 상세 내용을 저장할 **contents** 와 이미지를 저장할 **images** 두 가지 상태가 존재합니다.

#### CollectionPage.tsx

```typescript
  return (
    <section>
      <div>
        <h1>수거 신청하기</h1>
        <h4>의류를 보내서 포인트도 얻고 친환경도 실천해보세요!</h4>
      </div>
      {Array(formCount)
        .fill(0)
        .map((_, index) => (
          <div key={index}>
            <button onClick={() => setFormCount(formCount - 1)}>삭제</button>
            <CollectionForm
              images={images}
              setImages={setImages}
              contents={contents}
              setContents={setContents}
              index={index++}
            />
          </div>
        ))}
      <div>
        <button
          onClick={() => {
            setFormCount(formCount + 1);
            setContents([...contents, initialValue]);
          }}
        >
          상품 추가
        </button>
      </div>
      <div>
        <button onClick={() => submitHandler()}>보내기</button>
      </div>
    </section>
  );
};
```

- 그리고 작성 폼은 상품 추가 버튼을 클릭하면 **formCount** 가 1 증가하고 formCount만큼 **CollectionForm 컴포넌트**를 가져옵니다.
- 초기값은 1이므로 처음에는 하나의 폼만 그려내고 상품 추가 버튼을 누르면 formCount = 2가 되므로 두 개의 폼이 생기는 방식입니다.
- 이 때 index를 넘겨주어서 폼을 구분하였습니다.

#### CollectionForm.tsx

```typescript
interface Props {
  images: File[];
  setImages: React.Dispatch<React.SetStateAction<File[]>>;
  contents: ContentsProps[];
  setContents: React.Dispatch<React.SetStateAction<ContentsProps[]>>;
  index: number;
}

const CollectionForm = ({
  images,
  setImages,
  contents,
  setContents,
  index,
}: Props) => {
  const [preview, setPreview] = useState<string>();
  const [titleValue, titleHandler, titleReset] = useInput("");
  const [contentValue, contentHandler, contentReset] = useInput("");
  const [categoryValue, setCategoryValue] = useState("");

  useEffect(() => {
    setContents(
      contents.map((item, idx) =>
        index === idx
          ? {
              ...item,
              name: titleValue,
              content: contentValue,
              category: categoryValue,
            }
          : item
      )
    );
  }, [titleValue, contentValue, categoryValue]);

  const uploadImage = (e: React.ChangeEvent<HTMLInputElement>) => {
    const imageFile = e.target.files;
    if (imageFile && imageFile[0]) {
      const url = URL.createObjectURL(imageFile[0]);

      setPreview(url);
    }

    imageFile && setImages([...images, imageFile[0]]);
  };
```


#### CollectionForm.tsx

```typescript
interface Props {
  images: File[];
  setImages: React.Dispatch<React.SetStateAction<File[]>>;
  contents: ContentsProps[];
  setContents: React.Dispatch<React.SetStateAction<ContentsProps[]>>;
  index: number;
}

const CollectionForm = ({
  images,
  setImages,
  contents,
  setContents,
  index,
}: Props) => {
  const [preview, setPreview] = useState<string>();
  const [titleValue, titleHandler, titleReset] = useInput("");
  const [contentValue, contentHandler, contentReset] = useInput("");
  const [categoryValue, setCategoryValue] = useState("");
```
- **CollectionForm** 은 이미지 미리보기를 위한 **preview**와 콘텐츠의 각 내용들을 저장할 **value** 들이 있습니다.

#### CollectionForm.tsx

```typescript
  useEffect(() => {
    setContents(
      contents.map((item, idx) =>
        index === idx
          ? {
              ...item,
              name: titleValue,
              content: contentValue,
              category: categoryValue,
            }
          : item
      )
    );
  }, [titleValue, contentValue, categoryValue]);
```

- value값이 바뀌면 props로 전달받은 index와 같은 컴포넌트의 값을 변경합니다. **(setContents)**

#### CollectionForm.tsx

```typescript
  const uploadImage = (e: React.ChangeEvent<HTMLInputElement>) => {
    const imageFile = e.target.files;
    if (imageFile && imageFile[0]) {
      const url = URL.createObjectURL(imageFile[0]);

      setPreview(url);
    }

    imageFile && setImages([...images, imageFile[0]]);
  };
```

- 이미지는 따로 **setImages**에 저장을 합니다.

### 문제점

- 여기까지 구현을 했을 때 등록하는 부분에 한해서는 서버와 통신이 가능했습니다.
- 그러나 서버에 전달을 할 때 **images** 배열과 **contents** 배열을 따로 전달하는데 두 배열의 순서가 일치하지 않으면 잘못된 요청이 전달되었습니다.
- 예를 들어서 상품 추가 버튼을 눌러서 2개의 폼이 있는 상태에서 두 번째 폼에 이미지를 먼저 등록하고 그 다음으로 첫 번째 폼에 이미지를 등록한 후 상세 내용을 입력하면 서버에서는 두 번째 폼에 등록한 이미지를 1번 폼의 상세내용과 연결하게 되므로 원하는 결과를 얻을 수 없었습니다.
- 또한 여러 개의 폼 중 중간 혹은 맨 앞에 있는 폼을 삭제하게 되면 인덱스가 하나씩 변하게 되므로 이 또한 원하는 대로 요청을 보낼 수가 없었습니다.

### 고민한 방법

- 인덱스로 물품을 안정적으로 관리하는 것은 위험한 방법이라고 판단해서 각각 물품마다 **itemId** 를 부여하기로 하였습니다.
- 또한 formData로 전송을 할 때 contents와 image를 각각 다른 key값으로 분리해서 보내긴 하지만 관리할 때는 하나의 객체로 관리하는 것이 안정적이고 직관적이라고 생각하였습니다.

### 문제를 해결한 로직

#### CollectionPage.tsx

```typescript
export interface ItemProps {
  name: string;
  content: string;
  category: string;
}

export interface ContentsProps {
  itemId: number;
  itemInfo: ItemProps;
  itemImage: File | any;
}

const CollectionPage = () => {
  const initialValue = {
    itemId: 0,
    itemInfo: { name: "", content: "", category: "" },
    itemImage: undefined,
  };

  const [contents, setContents] = useState<ContentsProps[]>([initialValue]);
  const [itemNumber, setItemNumber] = useState(1);
```

- 우선 images와 contents로 분리되어 있던 상태를 하나로 합쳤습니다.
- 하나의 contents에는 **itemId**와 상세 내용을 저장하는 **itemInfo** 그리고 이미지를 저장하는 **itemImage**가 존재합니다.

```typescript
      <S.AddBtnBox>
        <S.AddFormBtn
          onClick={() => {
            setContents([
              ...contents,
              {
                itemId: itemNumber,
                itemInfo: { name: "", content: "", category: "" },
                itemImage: undefined,
              },
            ]);
            setItemNumber(itemNumber + 1);
          }}
        >
          상품 추가
        </S.AddFormBtn>
      </S.AddBtnBox>
```

- 또한 itemId는 **itemNumber**라는 상태를 이용해서 상품 추가 버튼을 누를 때마다 itemNumber가 1씩 증가하여 itemId에 부여되도록 하였습니다.

#### CollectionForm.tsx

```typescript
interface Props {
  contents: ContentsProps[];
  setContents: React.Dispatch<React.SetStateAction<ContentsProps[]>>;
  itemNumber: number;
}

const CollectionForm = ({ contents, setContents, itemNumber }: Props) => {
  const [preview, setPreview] = useState<string>();
  const [titleValue, titleHandler] = useInput("");
  const [contentValue, contentHandler] = useInput("");
  const [categoryValue, setCategoryValue] = useState("");
  const [imageFile, setImageFile] = useState<File>();

```

- **CollectionForm 컴포넌트**는 이제 콘텐츠와 이미지를 하나의 props로 받게 됩니다.

#### useInput.tsx

```typescript
import { ChangeEvent, useCallback, useState } from "react";

type UseInputProps<T> = [
  value: T,
  changeHandler: (
    e: ChangeEvent<HTMLInputElement> | ChangeEvent<HTMLTextAreaElement>
  ) => void,
  reset?: () => void
];

const useInput = <T>(initialValue: T): UseInputProps<T> => {
  const [value, setValue] = useState<T>(initialValue);

  const changeHandler = (e: ChangeEvent<HTMLInputElement> | any) => {
    if (e.target) setValue(e.target.value);
  };

  const reset = useCallback(() => setValue(initialValue), [initialValue]);

  return [value, changeHandler, reset];
};

export default useInput;
```

- 그리고 useInput에서 사용하지 않는 reset 함수 또한 삭제해주었습니다.

#### CollectionForm.tsx

```typescript
  useEffect(() => {
    setContents(
      contents.map(item =>
        item.itemId === itemNumber
          ? {
              ...item,
              itemId: itemNumber,
              itemInfo: {
                name: titleValue,
                content: contentValue,
                category: categoryValue,
              },
              itemImage: imageFile,
            }
          : item
      )
    );
  }, [titleValue, contentValue, categoryValue]);
```

- 기존에 props로 넘겨받은 index가 아니라 **itemNumber** 와 **itemId** 를 비교해서 값을 변경합니다. 인덱스가 변화해도 안정적으로 원하는 컴포넌트의 값만 변경할 수 있게 되었습니다.


## 반응형 적용하여 완성된 페이지

![](https://blog.kakaocdn.net/dn/GpbHI/btsnhTjA1DQ/sedPK44eYiU0qyObVFEBKk/img.gif)

## 전체 코드

#### CollectionPage.tsx

```typescript
import CollectionForm from "../../components/Collection_form/CollectionForm";
import { useState } from "react";
import axios from "axios";
import * as S from "./style";
import { BASE_URL } from "../../constants/constants";
import { useNavigate } from "react-router-dom";

export interface ItemProps {
  name: string;
  content: string;
  category: string;
}

export interface ContentsProps {
  itemId: number;
  itemInfo: ItemProps;
  itemImage: File | any;
}

const CollectionPage = () => {
  const initialValue = {
    itemId: 0,
    itemInfo: { name: "", content: "", category: "" },
    itemImage: undefined,
  };

  const [contents, setContents] = useState<ContentsProps[]>([initialValue]);
  const [itemNumber, setItemNumber] = useState(1);

  const navigate = useNavigate();

  const deleteHandler = (id: number) => {
    confirm("정말 삭제하시겠습니까?");
    setContents(contents.filter(item => item.itemId !== id));
  };

  const submitHandler = async () => {
    /* 예외 처리 */
    for (let i = 0; i < contents.length; i++) {
      if (
        contents[i].itemInfo.name === "" ||
        contents[i].itemInfo.content === "" ||
        contents[i].itemInfo.category === ""
      ) {
        alert("필수 항목을 모두 작성해주세요.");
        return;
      } else {
        if (contents[i].itemImage === undefined) {
          alert("이미지를 등록해주세요.");
          return;
        }
      }
    }

    /* 요청 보낼 데이터 처리 */
    const formData = new FormData();
    const contentList = [];

    for (let i = 0; i < contents.length; i++) {
      contentList.push(contents[i].itemInfo);
      contents[i].itemImage && formData.append("files", contents[i].itemImage);
    }

    formData.append("productlist", JSON.stringify(contentList));

    // API 요청 로직

    if (res.status === 200) {
      alert("정상적으로 요청되었습니다.");
      navigate("/");
    }
  };

  return (
    <S.Section>
      <S.PageTitle>
        <h1>수거 신청하기</h1>
        <h4>의류를 보내서 포인트도 얻고 친환경도 실천해보세요!</h4>
      </S.PageTitle>
      <S.ContentsContainer>
        {contents.map((item, index) => (
          <div key={item.itemId}>
            <S.ContentHeader>
              <div className="product_no">상품 번호 : {index + 1}</div>
              <S.DeleteBtn
                onClick={() => {
                  deleteHandler(item.itemId);
                }}
              >
                삭제
              </S.DeleteBtn>
            </S.ContentHeader>
            <CollectionForm
              contents={contents}
              setContents={setContents}
              itemNumber={item.itemId}
            />
          </div>
        ))}
      </S.ContentsContainer>
      <S.AddBtnBox>
        <S.AddFormBtn
          onClick={() => {
            setContents([
              ...contents,
              {
                itemId: itemNumber,
                itemInfo: { name: "", content: "", category: "" },
                itemImage: undefined,
              },
            ]);
            setItemNumber(itemNumber + 1);
          }}
        >
          상품 추가
        </S.AddFormBtn>
      </S.AddBtnBox>
      <S.SubmitBox>
        <div className="total_product">TOTAL : {contents.length}개의 물품</div>
        <S.SubmitBtn onClick={() => submitHandler()}>보내기</S.SubmitBtn>
      </S.SubmitBox>
    </S.Section>
  );
};

export default CollectionPage;
```


#### CollectionForm.tsx

```typescript
import { useEffect, useState, useRef } from "react";
import ImageIcon from "../../assets/icons/ImageIcon";
import * as S from "./style";
import { ContentsProps } from "../../pages/collectionPage/collectionPage";
import useInput from "../../hooks/useInput";
import SelectBox from "../SelectBox/SelectBox";

interface Props {
  contents: ContentsProps[];
  setContents: React.Dispatch<React.SetStateAction<ContentsProps[]>>;
  itemNumber: number;
}

const CollectionForm = ({ contents, setContents, itemNumber }: Props) => {
  const [preview, setPreview] = useState<string>();
  const [titleValue, titleHandler] = useInput("");
  const [contentValue, contentHandler] = useInput("");
  const [categoryValue, setCategoryValue] = useState("");
  const [imageFile, setImageFile] = useState<File>();
  const categoryOptions = ["상의", "하의", "아우터", "기타"];

  const imgInput = useRef<HTMLInputElement>(null);

  useEffect(() => {
    setContents(
      contents.map(item =>
        item.itemId === itemNumber
          ? {
              ...item,
              itemId: itemNumber,
              itemInfo: {
                name: titleValue,
                content: contentValue,
                category: categoryValue,
              },
              itemImage: imageFile,
            }
          : item
      )
    );
  }, [titleValue, contentValue, categoryValue]);

  /**
   * [이미지 업로드]
   * setPreview : 미리보기 이미지 저장
   * setImageFile : 객체에 이미지 파일 저장
   */
  const uploadImage = (e: React.ChangeEvent<HTMLInputElement>) => {
    const imageFile = e.target.files;
    if (imageFile && imageFile[0]) {
      const url = URL.createObjectURL(imageFile[0]);

      setPreview(url);
    }

    imageFile && setImageFile(imageFile[0]);
  };

  const uploadBtnClickHandler = () => {
    imgInput.current && imgInput.current.click();
  };

  return (
    <S.FormContainer>
      <S.ContentContainer>
        <S.Imagebox>
          <div className="image_background">
            {preview ? <img src={preview} /> : <ImageIcon />}
          </div>
          <input
            type="file"
            accept="image/*"
            onChange={uploadImage}
            ref={imgInput}
            style={{ display: "none" }}
          />
          <button className="upload_btn" onClick={uploadBtnClickHandler}>
            이미지 등록
          </button>
        </S.Imagebox>
        <S.ContentBox>
          <SelectBox
            usage={"카테고리"}
            options={categoryOptions}
            setOption={setCategoryValue}
          />
          <input
            type="text"
            placeholder="상품명을 입력해주세요."
            value={titleValue}
            onChange={titleHandler}
            required
          />
          <textarea
            placeholder="1. 브랜드 2. 컬러 3. 사이즈 등을 상세하게 입력해주세요."
            value={contentValue}
            onChange={contentHandler}
            required
          ></textarea>
        </S.ContentBox>
      </S.ContentContainer>
    </S.FormContainer>
  );
};

export default CollectionForm;
```