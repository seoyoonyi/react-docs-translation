# useState

`useState` is a React Hook that lets you add a state variable to your component. <br/>
"`useState`는 당신의 컴포넌트에 상태 변수를 추가할 수 있게 해주는 React Hook 입니다"

```javascript
const [state, setState] = useState(initialState);
```

# Reference

"참고"

## useState(initialState)

Call `useState` at the top level of your component to declare a state variable. <br/>
"상태 변수를 선언하기 위해서 컴포넌트의 최상위 레벨에서 `useState`를 호출하세요."

```javascript
import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());
  // ...
```

The convention is to name state variables like `[something, setSomething]` using array destructuring. <br/>
"규칙은 상태 변수 이름을 `[something, setSomething]` 처럼 짓되, 배열 구조 분해 할당을 사용하는 것이다."

see more examples below. <br/>
"아래에서 더 많은 예시를 확인하세요."

### Parameters

"매개 변수"

- `initialState`: <br/>
  The value you want the state to be initially. <br/>
  "`initialState`: 초기 상태로 설정하고자 하는 값입니다." <br/>
  It can be a value of any type, but there is a special behavior for functions. <br/>
  "이 값은 어떤 타입이든 설정할 수 있찌만, 함수인 경우에는 특별한 동작이 적용됩니다." <br/>
  This argument is ignored after the initial render. <br/>
  "또한, 이 인자는 초기 렌더링 이후에는 무시됩니다." 

- If you pass a function as `initialState`, it will be treated as an initializer function. <br/>
  "만약 `initialState`로 함수를 전달하면, 그것은 초기화 함수로 간주됩니다." <br/>
  It should be pure, should take no arguments, and should return a value of any type. <br/>
  "순수 함수여야 하며, 인수를 받지 않아야 하고, 어떤 타입의 값이라도 반환할 수 있어야 합니다."   <br/>
  "* 순수함수란 외부 상태에 영향을 주거나 의존하지 않고, 동일한 입력에 대해 항상 동일한 출력을 반환하는 함수입니다." <br/>
  React will call your initializer function when initializing the component, and store its return value as the initial state. <br/>
  "React는 컴포넌트를 초기화할 때 초기화 함수를 호출하고, 그(initializer function) 반환값을 초기 상태로 저장합니다." <br/>
  See an example below. <br/>
  "아래에서 더 많은 예시를 확인하세요."

### Returns

`useState` returns an array with exactly two values

1. The current state. During the first render, it will match the `initialState` you have passed.
2. The `set` function that lets you update the state to a different value and trigger a re-render.

### Caveats

- `useState` is a Hook, so you can only call it at the top level of your component or your own Hooks.
  You can't call it inside loops or conditions.
  If you nedd that, extract a new component and move the state into it.

- In Strict Mode, React will call your initializer function twice in order to help you find accidental impurities.
  This is development-only behavior and does not affect production.
  If your initializer function is pure (as it should be), this should not affect the behavior.
  The result from one of the calls will be ignored.

## 모르는 단어 정리

| **단어/표현**     | **뜻**         | **예문**                                                    | **나만의 설명**                          |
| ----------------- | -------------- | ----------------------------------------------------------- | ---------------------------------------- |
| **destructuring** | 구조 분해 할당 | like `[something, setSomething]` using array destructuring. | 배열이나 객체의 값을 쉽게 추출하는 문법. |