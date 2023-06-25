# 개요

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdhSQJ6%2FbtskZrxNHUo%2F9WqIGdhJAJqSYmNsTxh0J1%2Fimg.png)
- **react-quill**을 이용해서 텍스트 에디터를 구현하였고 **highlight.js** 라이브러리를 추가로 사용해서 텍스트 에디터 내의 코드블럭에 하이라이트 효과를 주었다.


# 문제 상황

![](https://blog.kakaocdn.net/dn/SCINy/btsk5R2i0JM/uOp2DuFJMmNoUJRyemVTjk/img.gif)

- 코드 블럭을 이용해서 글을 작성하거나 수정할 때 에디터 내에서 **onChange 이벤트를 발생시킬 무언가**를 하지 않으면 하이라이트 효과가 적용되지 않는 문제점이 있었다.
- 심지어 이미 하이라이트 효과가 적용되어 있는데도 Edit 버튼을 눌러서 아무것도 하지 않고 적용을 누르면 하이라이트 효과가 사라져버린다.

## 예상되는 원인

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbabrOS%2FbtskZrLkPL2%2FyR9f3gcDDBM0GpJusq8o1K%2Fimg.png)

- 우선 하이라이트 효과가 적용된 코드와 적용되지 않은 코드는 약간 차이가 있다.
- 위 이미지에서 윗부분이 하이라이트 효과가 적용된 것이라 class가 좀 더 세분화되어 있다.
- 글을 작성하거나 수정할 때 코드 블럭을 사용하면 잠시 뒤에 하이라이트 효과가 적용되는 것을 볼 수 있는데

#### TextEditor.tsx

```typescript
  return (
    <ReactQuill
      modules={modules}
      onChange={(e: any) => changeHandler(e)}
      value={bodyValue}
      style={{
        height: "210px",
      }}
    />
  );
```

#### useInput.tsx

```typescript
import { useState, useCallback, ChangeEvent } from "react";

type UseInputProps<T> = [
  value: T,
  changeHandler: (e: ChangeEvent<HTMLInputElement>) => void,
  reset: () => void
];

function useInput<T>(initialValue: T): UseInputProps<T> {
  const [value, setValue] = useState<T>(initialValue);

  const changeHandler = (e: ChangeEvent<HTMLInputElement> | any) => {
    if (e.target) setValue(e.target.value);
    else setValue(e);
  };

  const reset = useCallback(() => setValue(initialValue), [initialValue]);

  return [value, changeHandler, reset];
}

export default useInput;
```

### \[질문을 등록할 때\]

1. 텍스트 에디터 내에 코드 블럭을 복사 + 붙여넣기한다.
2. onChange 이벤트에 의해 코드가 setValue된다.
3. 잠시 뒤 코드 블럭에 하이라이트 효과가 적용된다.
4. 실제 코드는 변경되었지만 **onChange 이벤트를 감지하지 못한 상태** 로 value값은 여전히 하이라이트 효과가 적용되지 않은 상태이다.
5-1. 이 때 onChange 이벤트를 적용하는 무언가를 하면 하이라이트 효과가 적용된 값으로 setValue가 된다.
5-2. 아무것도 하지 않고 등록을 누르면 하이라이트 효과가 적용되지 않은 value값으로 데이터베이스에 등록이 된다.

### \[하이라이트 효과가 적용된 질문을 수정할 때\]

#### QuestionEdit.tsx

```typescript
function QuestionEdit() {
  const [initialTitle, setInitialTitle] = useState("");
  const [initialBody, setInitialBody] = useState("");
  const [titleValue, changeTitleHandler, titleReset] = useInput(initialTitle);
  const [bodyValue, changeBodyHandler, bodyReset] = useInput(initialBody);

  const navigate = useNavigate();

  const { id } = useParams();

  useEffect(() => {
    axios.get(`/api/questions/${id}`).then(res => {
      setInitialTitle(res.data.data.title);
      setInitialBody(res.data.data.content);
      titleReset();
      bodyReset();
    });
  }, [initialTitle, initialBody]);
						.
						.
						.
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAU2MR%2Fbtsk0EDf2Az%2FlEu6DkoLBnrFpzNCoFQU90%2Fimg.png)

1. 하이라이트 효과가 적용된 코드가 불러와진다.
2. onChange 이벤트로 현재 하이라이트가 적용되지 않은 상태의 코드로 값이 setValue 된다.
3. 하이라이트 효과가 적용된다.
4. 코드는 다시 하이라이트 효과가 적용된 상태로 바뀌었지만 onChange가 감지하지 못한다.


## 해결 방안

- 에디터에서 하이라이트 효과가 적용되었을 때 감지하도록 하거나 setTimeout같은 함수를 이용해서 하이라이트 효과가 적용되길 기다렸다가 setValue하는 방법이 떠올랐다.
- 