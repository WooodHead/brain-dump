# usePrefersReducedMotion

```javascript
const QUERY = "(prefers-reduced-motion: no-preference)";
const isRenderingOnServer = typeof window === "undefined";
const getInitialState = () => {
  // For our initial server render, we won't know if the user
  // prefers reduced motion, but it doesn't matter. This value
  // will be overwritten on the client, before any animations
  // occur.
  return isRenderingOnServer ? true : !window.matchMedia(QUERY).matches;
};
function usePrefersReducedMotion() {
  const [prefersReducedMotion, setPrefersReducedMotion] = React.useState(
    getInitialState
  );
  React.useEffect(() => {
    const mediaQueryList = window.matchMedia(QUERY);
    const listener = (event) => {
      setPrefersReducedMotion(!event.matches);
    };
    mediaQueryList.addListener(listener);
    return () => {
      mediaQueryList.removeListener(listener);
    };
  }, []);
  return prefersReducedMotion;
}
```

## Context

For some people, motion can be harmful.

The `prefers-reduced-motion` CSS media query allows us to disable animations for these folks. For our animations that are entirely in CSS (eg. CSS transitions, CSS keyframe animations), this works great 💯

What about for our animations in JavaScript, though? For example, when we use a library like React Spring or Framer Motion? We need to manage it ourselves, and it becomes a pretty tricky endeavour.

For these cases, I created a `use-prefers-reduced-motion` hook.

I wrote a blog post all about this:

[The “prefers-reduced-motion” Hook
A deep dive into how animations can be harmful, and what we can do to address it. Because nobody should feel sick after using our products!](https://joshwcomeau.com/react/prefers-reduced-motion/)

### Usage

### With React Spring

```jsx
import { useSpring, animated } from "react-spring";
const Box = ({ isBig }) => {
  const prefersReducedMotion = usePrefersReducedMotion();
  const styles = useSpring({
    width: 100,
    height: 100,
    background: "rebeccapurple",
    transform: isBig ? "scale(2)" : "scale(1)",
    immediate: prefersReducedMotion,
  });
  return <animated.div style={styles}>Box!</animated.div>;
};
```
