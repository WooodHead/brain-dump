# useToggle

```jsx
import React from "react";
export default function useToggle(initialValue = false) {
  const [value, setValue] = React.useState(initialValue);
  const toggle = React.useCallback(() => {
    setValue((v) => !v);
  }, []);
  return [value, toggle];
}
```

## Context

There are times when we want some React state that should always hold a boolean value, and should only allow toggling between true and false.

This is a thin wrapper around `React.useState` to solve for this purpose. It carries greater semantic meaning, and prevents us from accidentally setting a non-boolean value in the future.

### Usage

JSX

```jsx
const App = () => {
  const [isOn, toggleIsOn] = useToggle();
  return (
    <>
      {isOn ? "The light is on!" : "Hey who turned off the lights"}
      <button onClick={toggleIsOn}>Press me</button>
    </>
  );
};
```

The nice thing about this hook is that the `toggleIsOn` function can be passed straight to a click-handler; there's no need for an intermediary arrow that sets the value to the opposite of the value (and no opportunity for accidentally setting it to a non-boolean value!).

### Explanation

This hook uses `React.useCallback` in order to preserve the reference to the setter function; without this wrapper, the `toggle` function would be recreated on every render.

The cost of creating this function is minimal, but recreating it means that it would be a **_new reference_**. This would break [memoization of children elements](https://reactjs.org/docs/react-api.html#reactmemo).

For example, say we have this code:

```jsx
const SomeBigComponent = React.memo(({ toggleIsOn }) => {
  // Imagine lots of stuff here
});
const App = () => {
  const [isOn, toggleIsOn] = useToggle();
  const [count, setCount] = React.useState(0);
  return (
    <>
      The lights are {isOn ? "on" : "off"} and you've clicked {count} time(s).
      <button onClick={() => setCount((s) => s + 1)}>Increment</button>
      <SomeBigComponent toggleIsOn={toggleIsOn} />
    </>
  );
};
```

A button can be clicked to increment a counter bit of state. Whenever that button is clicked, `App` will re-render.

Because `SomeBigComponent` is wrapped with `React.memo`, it will only re-render when its props change. And it only has one prop, the `toggleIsOn` function.

Without the `React.useCallback` in our `useToggle` hook, that `toggleIsOn` function would change on every render! This means that `SomeBigComponent` would re-render even when the user is incrementing the count by clicking the button.

In practice, this likely won't be a huge performance problem in **_most_** cases, but because `useToggle` is generic and reusable, it doesn't hurt to bake this in.
