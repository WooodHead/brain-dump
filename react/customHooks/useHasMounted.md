# useHasMounted

```javascript
function useHasMounted() {
  const [hasMounted, setHasMounted] = React.useState(false);
  React.useEffect(() => {
    setHasMounted(true);
  }, []);
  return hasMounted;
}
```

> Note that this hook is useful for telling if the app has mounted, not whether the app is mounted. Having an \_isMounted is often considered an antipattern. In the cases where it is required (for example, when using not-cancellable promises), a ref works better than this hook.

## Context

When dealing with a server-side rendered application (through frameworks like Gatsby or Next, or any sort of SSR setup), it can be useful to know whether you're rendering on the server or the client.

For example: If you don't have a dynamic backend (eg. a Gatsby app), you won't know whether the user is authenticated or not. In that first pre-render, you should render a blank spot. When the cient renders, you can fill it in, either with the user's avatar or a "Sign in" link.

### Immediate renders

Immediately after the very first render, we set some state, which immediately triggers a re-render. This has a performance cost, but it's necessary.

Some developers try to get around this "double-render" by using a ref instead of state, to check whether they're on the server in the first render:

```javascript
const SomeClientSideOnlyComponent = () => {
  const isRenderingOnServer = typeof window === "undefined";
  // 🙅‍♀️ Don't do this:
  if (isRenderingOnServer) {
    return null;
  }
  return <div>Client only!</div>;
};
```

This is a **_bad idea_**; it can lead to really wonky bugs.

For more information, check out [The Perils of Rehydration.](https://joshwcomeau.com/react/the-perils-of-rehydration) It explains why this is a bad idea in much greater depth.

## Usage

```javascript
const SomeClientSideOnlyComponent = () => {
  const hasMounted = useHasMounted();
  if (!hasMounted) {
    return null;
  }
  return <div>Client only!</div>;
};
```
