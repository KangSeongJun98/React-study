# 1. useState
### 여러개의 input태그 관리
```
import React, { useState } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    name: '',
    nickname: ''
  });

  const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

  const onChange = (e) => {
    const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
    setInputs({
      ...inputs, // 기존의 input 객체를 복사한 뒤
      [name]: value // name 키를 가진 값을 value 로 설정
    });
  };

  const onReset = () => {
    setInputs({
      name: '',
      nickname: '',
    })
  };


  return (
    <div>
      <input name="name" placeholder="이름" onChange={onChange} value={name} />
      <input name="nickname" placeholder="닉네임" onChange={onChange} value={nickname}/>
      <button onClick={onReset}>초기화</button>
      <div>
        <b>값: </b>
        {name} ({nickname})
      </div>
    </div>
  );
}

export default InputSample;
```
1. inputs의 초기값을 객체로 선언 <br />
2. name과 nickname을 비구조화로 할당 <br />
3. event객체의 값을 가져온다. <br />
4. set함수에서 event객체의 값을 key로 갖는 inputs의 값을 변경해준다.<br /> 

# 2. useRef
### React의 동작과정
1. 컴포넌트 렌더링: React 애플리케이션은 루트 컴포넌트에서 시작하여 하위 컴포넌트를 재귀적으로 렌더링

2. Virtual DOM 생성: JSX를 사용하여 컴포넌트를 정의하면 React는 이를 JavaScript 객체인 Virtual DOM으로 변환. 이 객체는 실제 DOM의 가벼운 복제본으로, DOM 업데이트를 추상화하고 성능을 개선하는 데 도움을 준다.

3. 상태 변경 감지: 컴포넌트의 상태(state)나 속성(props)이 변경되면 React는 해당 컴포넌트의 render() 메소드를 호출하여 새로운 Virtual DOM을 생성.

4. Virtual DOM 비교 및 업데이트: 이전 Virtual DOM과 새로운 Virtual DOM을 비교하여 실제 DOM에서 변경이 필요한 부분을 식별. 이후 변경된 부분만을 선택적으로 실제 DOM에 업데이트한다.
<hr>

### useRef의 사용처
- useRef는 실제 Dom의 요소를 직접 선택할 때 사용한다.
- React는 useState의 set함수를 사용하면 페이지가 리랜더링 된다.(view페이지의 값이 변경)
- useRef로 생성한 변수는 값이 변경되어도 컴포넌트가 리렌더링되지 않는다.

#### 사용법
```
import React, { useRef } from 'react';

const FocusableInput = () => {
  // useRef를 사용하여 input 요소의 참조를 생성
  const inputRef = useRef(null);

  // 버튼을 클릭할 때 input 요소에 포커스를 설정하는 함수
  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      {/* useRef로 생성한 ref를 input 요소의 ref 속성에 할당 */}
      <input type="text" ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
};

export default FocusableInput;
```
1. const inputRef = useRef(null)로 선언 매개변수는 초기값
2. input태그의 ref 속성에 변수 할당
3. 버튼을 눌렀을 때 input태그가 focus되도록 함
4. ref로 useRef()의 변수를 할당하면 해당 자식을 포함한 해당 요소 전체가 담긴다.
5. Dom 요소에 직접 접근하는것 말고 단순히 변경된 값을 저장하기 위해 사용되는 경우가 있으며<br />
이 때는 ref에 값을 주지 않아도 됨(ref는 필수가 아님)

# 3. useEffect
- 페이지가 랜더링 될 때 실행된다.
- 의존성 배열을 추가하면 배열안의 값이 바뀔 때마다 함수가 실행된다.
- useEffect함수가 컴포넌트 안에 정의돼있으면 알아서 랜더링, 변수 값 바뀔 때마다 실행된다.
### 예시 
```
 useEffect(() => {
    console.log(useEffect실행);
  }, [firstCount]);
```
1. useEffect의 매개변수는 두 개 함수와, 의존성 배열이다.
2. firstCount 변수의 값이 바뀔 때마다 console.log함수가 실행
3. 페이지가 랜더링 될 때 console.log 가 실행