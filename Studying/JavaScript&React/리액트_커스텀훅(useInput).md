# 개요

- 스택오버플로우를 클론 코딩하는 Pre-project 팀 과제를 진행하던 중, 질문 상세 페이지와 수정 페이지, 답변 수정 페이지에서 중복되는 코드가 많이 발생하였다.

![](https://blog.kakaocdn.net/dn/GZSn6/btskQhVEUcn/Uw4Zre7e43jauodzKmKZgk/img.gif)


# 질문 등록 페이지

## AskQuestion

```typescript
function AskQuestion() {
  const [titleValue, setTitleValue] = useState<string>("");
  const [bodyValue, setBodyValue] = useState<string>("");

  const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setBodyValue("");
  };

  return (
    <S.Section>
      <S.QuestionLayout>
        <S.Header>
          <S.QuestionTitle>Ask a public question</S.QuestionTitle>
          <S.TitleImage />
        </S.Header>
        <QuestionNotice />
        <S.FormLayout onSubmit={e => submitHandler(e)}>
          <QuestionTitle setTitleValue={setTitleValue} />
          <QuestionBody setBodyValue={setBodyValue} />
          <S.ButtonLayout>
            <button type="submit">Post your question</button>
            <Link to="/">
              <button>Discard draft</button>
            </Link>
          </S.ButtonLayout>
        </S.FormLayout>
      </S.QuestionLayout>
    </S.Section>
  );
}

export default AskQuestion;
```

- 질문 등록 페이지에서는 제목과 내용에 들어갈 state와 submitHandler를 가지고 있다.
- 그리고 각각 **QuestionTitle**과 **QuestionBody** 컴포넌트에 값의 상태를 변화시킬 setState 함수를 props로 내려준다.

# 컴포넌트

## QuestionTitle

```typescript
<S.InputTitle
	type="text"
	placeholder="e.g. Is ther R function for finding the index of an element in a vector?"
	onChange={e => setTitleValue(e.target.value)}
	autoFocus
  />
```

- onChange로 값이 바뀔 때마다 setState 함수를 이용해 저장하게 된다.

## QuestionBody

```typescript
  return (
    <S.Container>
      <S.InputBodyLayout>
        <S.SubHeading>Body</S.SubHeading>
        <S.SubContent>
          The body of your question contains your problem details and results.
          Minimum 220 characters.
        </S.SubContent>
        <TextEditor setBodyValue={setBodyValue} />
      </S.InputBodyLayout>
      <QuestionTip TipTitle={TipTitle} TipText={TipText} />
    </S.Container>
  );
}
```

- 질문의 내용이 들어가는 **QuesitonBody** 컴포넌트에서는 Input 타입이 아니라 텍스트 에디터(React-Quill)를 사용하는데 텍스트 에디터는 재사용을 많이 할 컴포넌트이기 때문에 따로 분리하였다.
- 그래서 setState 함수를 다시 **TextEditor** 컴포넌트에 내려주게 된다.

## TextEditor

```typescript
import ReactQuill from "react-quill";
import "react-quill/dist/quill.snow.css";

interface EditorProps {
  setBodyValue: React.Dispatch<React.SetStateAction<string>>;
}

function TextEditor({ setBodyValue }: EditorProps) {
  const onChangeHandler = (e: any) => {
    setBodyValue(e);
  };

  return <ReactQuill onChange={onChangeHandler} style={{ height: "210px" }} />;
}

export default TextEditor;
```

- **TextEditor** 컴포넌트에서는 **QuestionTitle** 컴포넌트에서처럼 값이 바뀔 때마다 setState를 통해 새로운 값을 저장하게 된다.

# 문제점

- 질문 등록을 할 때 위와 같은 로직을 따르는데 질문 수정, 답변 등록, 답변 수정을 할 때 각 페이지마다 위와 마찬가지로 **useState**를 그대로 남발하게 된다.

## QuestionEdit

```typescript
function QuestionEdit() {
  const [titleValue, setTitleValue] = useState<string>("");
  const [bodyValue, setBodyValue] = useState<string>("");

 const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
e.preventDefault();
setTitleValue("");
setBodyValue("");
};

  return (
	<S.FormLayout onSubmit={e => submitHandler(e)}>
	  <div>
		<S.TitleBox>
		  <S.SubHeading>Title</S.SubHeading>
		  <S.InputTitle
			type="text"
			placeholder="e.g. Is ther R function for finding the index of an element in a vector?"
			value={titleValue}
			onChange={e => setTitleValue(e.target.value)}
		  />
		</S.TitleBox>
			<div>
			<S.SubHeading>Body</S.SubHeading>
			<TextEditor setBodyValue={setBodyValue} />
			<S.Viewer
				  dangerouslySetInnerHTML={{
					__html: bodyValue,
				  }}
				/>
		</div>
```


## Answer(등록)

```typescript
function Answer({ answerData }: Props) {
  const [bodyValue, setBodyValue] = useState<string>("");

  const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setBodyValue("");
  };
  
  return (
						  .
						  .
						  .
	<S.FormLayout>
        <S.FormBox onSubmit={e => submitHandler(e)}>
          <div>Your Answer</div>
          <TextEditor setBodyValue={setBodyValue} />
          <S.ButtonLayout>
            <button type="submit">Post your answer</button>
          </S.ButtonLayout>
        </S.FormBox>
    </S.FormLayout>
```


## AnswerEdit

```typescript
function AnswerEdit() {
  const [bodyValue, setBodyValue] = useState("");

  const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
	e.preventDefault();
	setBodyValue("");
};

  return (
						.
						.
						.
				
	  <div
		dangerouslySetInnerHTML={{
		  __html: question.content,
		}}
	  />
	  <S.Grippie></S.Grippie>
	</div>
	<S.SubHeading>Answer</S.SubHeading>
	<TextEditor setBodyValue={setBodyValue} />
	<S.Viewer
	  dangerouslySetInnerHTML={{
		__html: bodyValue,
	  }}
	/>

```

- 이러한 중복과 낭비를 없애기 위해 **useInput**이라는 CustomHook을 직접 만들어보기로 하였다!

# UseInput 개발

- 훅을 만들기 전에, 중복이 되는 부분과 가공이 되고난 뒤 필요한 부분이 무엇인지를 파악해야 했다.

## 중복 되는 부분 파악

- 먼저 값의 상태를 변화시키는 **useState** 그리고 state 값을 변화시키는 **changeHandler** 가 useState가 위치하는 훅에 있으면 좋을 것 같다는 생각을 하였다.

## 필요한 부분 파악

- useInput이라는 훅에서 가공되어 나온 새로운 값인 **value**가 필요할 것이고 각 input에서 onChange 속성을 사용할 것이므로 **changeHandler** 자체가 필요할 것 같다는 생각을 하였다.

### useInput

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

- value값은 string만 들어올 것이 확실하지만 혹시나 scope가 확장되어서 다른 타입을 받아야할 수도 있기 때문에 제네릭 \<T\> 로 주었다.
- 그리고 개발을 하며 알게된 점은, submitHandler에서 submit을 한 뒤에 textArea의 값을 비워주기 위해 **setState("")** 를 하는 부분이 있는데 더이상 컴포넌트에서 State를 사용하지 않기 때문에 이 또한 훅에서 처리해주어야 했다.
- 그래서 값을 초기값으로 되돌리는 reset 이라는 함수 또한 훅에 추가하였다!
- 또한 텍스트 에디터인 **React-quill**은 onChange 속성에서 다른 input값들과 달리 target.value가 아닌 value를 그대로 반환하기 때문에 조건문을 사용해서 분기하였다.

### askQuestion

```typescript
function AskQuestion() {
  const [titleValue, changeTitleHandler, TitleReset] = useInput("");
  const [bodyValue, changeBodyHandler, BodyReset] = useInput("");

	const submitHandler = (e: React.FormEvent<HTMLFormElement>) => {
	    e.preventDefault();
	    TitleReset();
	    BodyReset();
  };

	return (
					.
					.
		<S.FormLayout onSubmit={e => submitHandler(e)}>
          <QuestionTitle
            changeHandler={changeTitleHandler}
            titleValue={titleValue}
          />
          <QuestionBody
            changeHandler={changeBodyHandler}
            bodyValue={bodyValue}
          />
	)
```

- 더이상 useState를 사용하지 않고 useInput에서 받는 값과 핸들러를 그대로 내려준다.
- 그리고 submit 후 useInput 안에 있는 reset 함수를 호출해서 텍스트창을 비워주게 된다.

### QuestionTitle

```typescript
function QuestionTitle({
  changeHandler,
  titleValue,
}: TitleProps) {
  return (
					  .
					  .
					  .
	<S.InputTitle
		type="text"
		placeholder="e.g. Is ther R function for finding the index of an element in a vector?"
		onChange={changeHandler}
		value={titleValue}
		autoFocus
	  />
```

- 값이 바뀌면 useInput 안에 있는 핸들러를 호출해서 값을 변화시키고 그 안에서 가공된 value를 그대로 받아서 업데이트하게 된다.


# 개선해야 되는 점

- 질문이나 답변을 수정하기 위해 Edit 버튼을 눌렀을 때 사용자UX 측면에서 기존에 등록했던 내용이 초깃값으로 그대로 노출이 되어야 한다.
- 그래서 useInput을 호출할 때 initialValue를 주었던 것인데 
- 문제는 처음에 렌더링이 될 때 데이터를 받아와서 그 데이터를 그대로 useInput의 초깃값으로 주어도 수정 작업에 의해 changeHandler가 호출되지 않는 이상 그 자체로는 useInput이 호출되지 않기 때문에 기존 내용이 노출되지 않는다.

![](https://blog.kakaocdn.net/dn/ctdbqA/btskT1yuilY/3iKP9HW2HWhTd5d7uhwmh1/img.gif)

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
```

- 그래서 또 다시 useState를 사용해서 데이터를 받아온 뒤에 그 데이터를 초기값으로 주고 reset 함수를 실행함으로써 useInput을 호출하도록 하였다.

![](https://blog.kakaocdn.net/dn/nlh2N/btskZsBNd0f/Bxe6GxKZrkZMb5VKu45uK1/img.gif)

- 일단 해결 자체는 되었는데 useState의 사용이 단순히 초기값을 주기위한 용도로 사용되었다는 점이 마음에 걸린다 ㅠㅠ
- 더 좋은 방법이 있을 것 같아서 어떻게 개선해야 할지 고민이 된다고 할 수 있겠다.