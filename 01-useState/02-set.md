# `set` functions, like `setSomething(nextState)`
"`set`함수, 예: `setSomething(nextState)`"

The set function returned by useState lets you update the state to a different value and trigger a re-render. <br/>
"useState로 반환된 set 함수는 상태를 다른 값으로 업데이트하고 컴포넌트를 다시 렌더링하도록 트리거 합니다"  <br/>
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
- nextState: The value that you want the state to be. It can be a value of any type, but there is a special behavior for functions.
  - if you pass a functtion as `nextState`, it will be treated as an updater function. It must be pure, should take the pending state as its only argument, and should return the next state.
  React will put your updater function in a queue and re-render your component.
  During the next render, React will calculate the next state by applying all of the queued updaters to the previous state.
  See an example below.

## Returns
set functions do not have a return value.

## Caveats
- The `set` function only updates the state variable for the next render. If you read the state variable after calling the set function, you will still get the old value that was on the screen before your call.
- If the new value you provide is identical to the current `state`, as determined by an `Object.is` comparison, React will skip re-rendering the component and its children. This is an optimization.
Although in some cases React may still nedd to call your component before skipping the children, it shouldn't affect your code.
- React batches state updates. It updates the screen after all the event handlers have run and have called their `set` functions. This prevents multiple re-renders during a single event. In the rare case that you nedd to force React to update the screen earlier, for example to access the DOM, you can use `flushSync`.

- 그 뒤로 3개 더 있음...

