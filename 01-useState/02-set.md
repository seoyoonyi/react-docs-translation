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

"주의사항"

- The `set` function only updates the state variable for the next render. <br/>
  "set 함수는 상태 변수를 다음 렌더링에서만 업데이트합니다." <br/>
  If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call. <br/>
  "set 함수를 호출한 후 상태 변수를 읽으면, 호출하기 전 화면에 표시되던 이전 값이 여전히 반환됩니다." <br/>
- If the new value you provide is identical to the current `state`, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. This is an optimization. <br/>
  "만약 제공한 새로운 값이 현재 `state`와 동일하다면, React는 `Object.is` 비교를 통해 이를 판단하고, 컴포넌트와 그 자식들의 재렌더링을 건너뛰게 됩니다. 이것이 최적화입니다." <br/>
  Although in some cases React may still need to call your component before skipping the children, it shouldn't affect your code. <br/>
  "비록 일부 경우에는 React가 자식 컴포넌트를 건너뛰기 전에 여전히 당신의 컴포넌트를 호출해야 할 수도 있지만, 이는 당신의 코드에 영향을 주지 않아야 합니다." <br/>
- React batches state updates. <br/>
  "React는 상태 업데이트를 하나로 묶어서 처리합니다." <br/>
  <br/>

  **설명** <br/>
  React에서 상태 업데이트를 한 번에 처리하는 방식으로, 여러 상태 업데이트를 묶어서 불필요한 렌더링을 줄이고 성능을 최적화하는 개념입니다.
  이벤트 핸들러 내부에서 여러 `setState` 호출이 발생해도, React는 이를 모아서 한 번만 렌더링합니다.
  React 18 이후로는 기본적으로 비동기적으로 처리되지만, batching 자체가 항상 비동기를 의미하지는 않습니다.

  ```javascript
  const handleClick = () => {
    setCount(count + 1); // 상태 업데이트 1
    setCount(count + 1); // 상태 업데이트 2
  };
  //상태 업데이트가 두 번 호출되었지만, React는 두 개의 업데이트를 묶어서 처리합니다.
  ```

  It updates the screen after all the event handlers have run and have called their `set` functions. <br/>
  "React는 모든 이벤트 핸들러가 실행되어 `set`함수들을 호출한 후에 화면을 업데이트합니다." <br/>
  This prevents multiple re-renders during a single event. <br/>
  "이는 단일 이벤트 중에 여러 번의 재렌더링이 발생하는 것을 방지합니다." <br/>
  In the rare case that you need to force React to update the screen earlier, for example to access the DOM, you can use `flushSync`. <br/>
  "드물게 React가 화면을 더 빨리 업데이트하도록 강제해야 하는 경우, 예를 들어 DOM에 접근하기 위해 `flushSync`를 사용할 수 있습니다." <br/>
  <br/>
  **설명** <br/>
  React에서 상태 업데이트가 비동기적으로 처리될 때, 특정 상황에서 **즉시 DOM 업데이트**가 필요하면 `flushSync`를 사용합니다. 일반적으로 React는 성능을 위해 상태 업데이트를 모아서 처리(batching)하지만, `flushSync`를 사용하면 해당 상태 업데이트를 즉각적으로 반영하고 렌더링합니다.

      ```javascript

  import { flushSync } from 'react-dom';

  flushSync(() => {
  setCount(count + 1); // 즉각적으로 상태 업데이트를 반영
  });

  ```

  ```

- The `set` function has a stable identity, so you will often see it omitted from Effect dependencies, but including it will not cause the Effect to fire. <br/>
  If the linter lets you omit a dependency without errors, it is safe to do. Learn more about removing Effect dependencies. <br/>
- Calling the `set` function during rendering is only allowed from within the currently rendering component. <br/>
  React will discard its output and immediately attempt to render it again with the new state. <br/>
  This pattern is rarely needed, but you can use it to store information from previous renders. See an example below. <br/>
- In Strict Mode, React will call your updater function twice in order to help you find accidental impurities. <br/>
  This is development-only behavior and does not affect production. <br/>
  If your updater function is pure (as it should be), this should not affect the behavior. <br/>
  The result from one of the calls will be ignored.

## 모르는 단어 정리

| **단어/표현**    | **뜻**                  | **예문**                                                                                          | **나만의 설명**                                                                                           |
| ---------------- | ----------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **treated**      | 취급되다, 간주되다      | If you pass a function as nextState, it will be treated as an updater function.                   | 어떤 것을 특정한 방식으로 다루거나 간주할 때 사용. 예: "업데이트 함수로 간주된다"는 뜻.                   |
| **queue**        | 대기열, 줄              | React will put your updater function in a queue and re-render your component.                     | 작업이나 요청을 차례대로 처리하기 위해 기다리는 줄이나 목록. 컴퓨터에서는 FIFO(First In, First Out) 방식. |
| **queued**       | 대기 중인, 줄 선        | React will calculate the next state by applying all of the queued updaters to the previous state. | queue(대기열)에 들어가 있거나, 처리되기를 기다리는 상태를 나타냄. 대기열에 들어간 상태.                   |
| **identical**    | 동일한, 똑같은          | If the new value you provide is identical to the current `state`                                  | 두 대상이 외형, 성질, 내용 등에서 완전히 같을 때 사용하는 표현.                                           |
| **determined**   | 결심한, 단호한 / 결정된 | as determined by an `Object.is` comparison                                                        | 목표를 이루기 위해 단호히 결심한 상태를 뜻하거나, 어떤 결론이 내려졌음을 의미.                            |
| **comparison**   | 비교, 비유              | as determined by an `Object.is` comparison                                                        | 두 대상의 공통점과 차이점을 분석하거나, 하나를 다른 것에 빗댈 때 사용하는 단어.                           |
| **optimization** | 최적화                  | This is an optimization.                                                                          | 시스템, 프로세스, 작업 등을 가장 효율적으로 만들거나 개선하는 것.                                         |
| **although**     | 비록 ~일지라도, 하지만  | Although in some cases React may still need to call your component before skipping the children,  | 서로 상반된 상황을 연결할 때 사용하는 접속사. "비록 ~일지라도" 또는 "하지만"의 뜻.                        |
| **batches**      | 묶음, 하나로 묶다       | React batches state updates.                                                                      | React에서 여러 상태 업데이트를 하나로 묶어서 처리하여 효율적으로 렌더링하는 것을 의미.                    |
| **prevents**     | 방지하다, 막다          | This prevents multiple re-renders during a single event.                                          | 어떤 일이 발생하지 않도록 사전에 막는 것을 의미. React에서는 불필요한 재렌더링을 방지하는 데 사용.        |
| **rare**         | 드문, 희귀한            | In the rare case that you need to force React to update the screen earlier,                       | 어떤 일이 자주 발생하지 않거나 흔치 않을 때 사용하는 표현.                                                |
