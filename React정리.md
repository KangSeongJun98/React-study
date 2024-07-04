# JS와 React의 차이점
- html에 class를 사용하고 싶으면 className이라고 쓴다.
- 변수나 함수를 넣고 싶으면 {}(중괄호)를 사용한다.
- 이벤트 리스너 등에 함수를 넣을 때 ()는 생략한다.
- form태그에서 retrun false; 를 줘도 기본 동작을 방지 못한다.<br>
이 때 함수를 만들어서 preventDefault를 사용해야 방지할 수 있다
### form태그 기본 동작 방지
```
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

# Component

- Component는 React의 기본 구성 요소
- 함수형과 클래스형 컴포넌트가 있다.
- JSX 문법을 사용
- 첫 글자를 대문자로 작성
- 작성 할 때 반드시 하나 이상의 부모가 필요.
- 부모 태그는 <>같은 빈 태그도 가능

### 컴포넌트 예시
```
const MyComponent = () => {
  return (
    <>
      <h1>Hello</h1>
      <p>This is a paragraph.</p>
    </>
  );
};
```
- 작성된 컴포넌트는 태그 처럼 사용
- 다른 컴포넌트에서 import해서 사용할 수도 있음
- return에 null을 줘서 컴포넌트가 표시 안되게도 할 수 있음 

### 컴포넌트에서 조건 걸기
1. 일반적인 if else
```
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

2. 삼항 연산자
```
<div>
  {isLoggedIn ? (<AdminPanel />) : (<LoginForm />)}
</div>
```

3. &&연산자 (else가 필요 없을 때)
```
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

### 컴포넌트에서 컴포넌트로 값 넘기기
- 부모 컴포넌트에서 자식 컴포넌트로 값 넘길 때<br>
name을 쓰는게 아니라 속성을 하나 만들어서 거기에 값을 담는다.
- 자식쪽 컴포넌트는 함수의 매개변수로 props.속성값 을 쓴다.
- 단 이때 부모 컴포넌트의 return값에 자식 태그가 있어야 한다
```
// 부모														
const Parent = () => {
  const [data, setData] = useState(0);
  const getData = childData => {
    setData(childData);
  };
  return (
    <div>
      <p>{data}</p>
      <Child getData={getData} />
    </div>
  );
};


// 자식
const Child = ({ getData }) => {
  const data = 100;
  const postData = () => {
    getData(data);
  };
  return (
    <div>
      <button onClick={postData}>
    	데이터 전송
      </button>
    </div>
  );
};

```
- React 코드를 확인 할 때 역순으로 따라 올라갈 것
- root.render가 실행되는 코드에서 역순으로 체크해가면서 렌더링 되는 것을 확인

# props
- props 컴포넌트의 매개변수에 들어가는 객체
- props 객체에는 속성은 물론 함수도 포함할 수 있음
- props의 직접 값을 바꾸는건 React에서 불가능 함

### props 값 변경
```
import React, { useState } from 'react';


const ParentComponent = () => {
  const [message, setMessage] = useState('Hello from Parent');

  const handleClick = () => {
    setMessage('Button Clicked!');
  };  
  
  return (
    <div>
      <h1>{message}</h1>
      <ChildComponent onClickHandler={handleClick} />
    </div>
  );
};

```
- useState를 import한 뒤에 사용
- 위의 코드에서는 message에 값이 setMessage에는 message의 값을 변경할 수 있는 함수가 담긴다.
- message = message +1 과 같은 방식은 불가능 반드시 setMessage 함수를 사용해야한다.

### props의 값을 연속적으로 변경할 때
- React는 함수가 전부 실행되기 전까지 랜더링을 하지 않는다.
- 매 번 즉각적으로 값이 변경되는 것을 반영하는 비효율적이기에 이전에 있는 값에다가 현재의 값을 덮어 씌운다.(콜백함수) 
- 나도 값을 연속적으로 변경하고 싶으면 콜백 함수를 써야한다.

#### 안되는 예시  
```
  const onClickCount = () => {
    setCount( count + 1);
    setCount( count + 1);
    setCount( count + 1);
    setCount( count + 1);
  };
```
- 이렇게 해도 결과적으론 count+1 밖에 안된다.

#### 올바른 예시
```
const onClickCount = () => {
  setCount(prevCount => prevCount + 1);
  setCount(prevCount => prevCount + 1);
  setCount(prevCount => prevCount + 1);
  setCount(prevCount => prevCount + 1);
};
```
- 콜백 함수를 사용해서 바꿔줘야 한다.
- 위는 화살표 함수를 사용해서 더 짧게 쓴 것 

# 배열
- map함수를 사용하면 배열에 값을 쉽게 사용할 수 있음 

1. 새로운 배열 만들기
```
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

2. 여러개의 컴포넌트 렌더링 하기
```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```