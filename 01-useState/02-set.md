# `set` functions, like `setSomething(nextState)`

"`set`함수, 예: `setSomething(nextState)`"

The set function returned by useState lets you update the state to a different value and trigger a re-render. <br/>
"useState로 반환된 set 함수는 상태를 다른 값으로 업데이트하고 컴포넌트를 다시 렌더링하도록 트리거 합니다" <br/>
You can pass the next state directly, or a function that calculates it from the previous state: <br/>
"개발자는 다음 상태(state)를 직접 전달하거나, 이전 상태(state)를 기반으로 이를 계산하는 함수를 전달할 수 있습니다."

```javascript
const [name, setName] = useState('Edward');

function handleClick() {
  setName('Taylor');
  setAge(a => a + 1);
  // ...
```

## Parameters

"매개 변수"

- nextState: The value that you want the state to be. It can be a value of any type, but there is a special behavior for functions. <br/>
  "nextState: 상태를 변경하려는 값입니다. 이 값은 어떤 유형이든 될 수 있지만, 함수인 경우에는 특별한 동작이 있습니다" <br/>
  - if you pass a function as `nextState`, it will be treated as an updater function. It must be pure, should take the pending state as its only argument, and should return the next state. <br/>
    "`nextState`에 함수를 전달하면, 해당 함수는 업데이트 함수로 처리됩니다." <br/>
    React will put your updater function in a queue and re-render your component. <br/>
    "React는 업데이트 함수를 대기열에 추가하고 컴포넌트를 다시 렌더링합니다." <br/>
    During the next render, React will calculate the next state by applying all of the queued updaters to the previous state. <br/>
    "다음 렌더링 시, React는 이전 상태에 모든 대기 중인 업데이트 함수를 적용하여 다음 상태를 계산합니다." <br/>
    See an example below. <br/>
    "아래에서 더 많은 예시를 확인하세요." <br/>

## Returns

"반환값"

set functions do not have a return value. <br/>
"set 함수는 반환값을 가지지 않습니다."

## Caveats

- The `set` function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call.
- If the new value you provide is identical to the current `state`, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. This is an optimization.
  Although in some cases React may still need to call your component before skipping the children, it shouldn't affect your code.
- React batches state updates. It updates the screen after all the event handlers have run and have called their `set` functions. This prevents multiple re-renders during a single event. In the rare case that you need to force React to update the screen earlier, for example to access the DOM, you can use `flushSync`.

- 그 뒤로 3개 더 있음...

## 모르는 단어 정리

| **단어/표현** | **뜻**             | **예문**                                                                                          | **나만의 설명**                                                                                           |
| ------------- | ------------------ | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **treated**   | 취급되다, 간주되다 | If you pass a function as nextState, it will be treated as an updater function.                   | 어떤 것을 특정한 방식으로 다루거나 간주할 때 사용. 예: "업데이트 함수로 간주된다"는 뜻.                   |
| **queue**     | 대기열, 줄         | React will put your updater function in a queue and re-render your component.                     | 작업이나 요청을 차례대로 처리하기 위해 기다리는 줄이나 목록. 컴퓨터에서는 FIFO(First In, First Out) 방식. |
| **queued**    | 대기 중인, 줄 선   | React will calculate the next state by applying all of the queued updaters to the previous state. | queue(대기열)에 들어가 있거나, 처리되기를 기다리는 상태를 나타냄. 대기열에 들어간 상태.                   |
