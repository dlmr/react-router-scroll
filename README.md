# react-router-scroll [![Travis][build-badge]][build] [![npm][npm-badge]][npm]

[React Router](https://github.com/reactjs/react-router) scroll management.

react-router-scroll is a React Router middleware that adds scroll management using [scroll-behavior](https://github.com/taion/scroll-behavior). By default, the middleware adds browser-style scroll behavior, but you can customize it to scroll however you want on route transitions.

## Usage

```jsx
import { applyRouterMiddleware, browserHistory, Router } from 'react-router';
import { useScroll } from 'react-router-scroll';

/* ... */

ReactDOM.render(
  <Router
    history={browserHistory}
    routes={routes}
    render={applyRouterMiddleware(useScroll())}
  />,
  container
);
```

## Guide

### Installation

```shell
$ npm i -S react react-dom react-router
$ npm i -S react-router-scroll
```

### Basic usage

Apply the `useScroll` router middleware, as in the example above.

### Custom scroll behavior

You can provide a custom `shouldUpdateScroll` callback as an argument to `useScroll`. This callback is called with both the previous and the current router props.

You can return:

- a falsy value to suppress the scroll update
- a position array such as `[0, 100]` to scroll to that position
- a truthy value to get normal scroll behavior

```jsx
useScroll((prevRouterProps, { location }) => (
  prevRouterProps && location.pathname !== prevRouterProps.location.pathname
));

useScroll((prevRouterProps, { routes }) => {
  if (routes.some(route => route.ignoreScrollBehavior)) {
    return false;
  }

  if (routes.some(route => route.scrollToTop)) {
    return [0, 0];
  }

  return true;
});
```

### Scrolling elements other than `window`

Use `<ScrollContainer>` to manage the scroll behavior of elements other than `window`. Each `<ScrollContainer>` must be given a unique `scrollKey`, and can be given an optional `shouldUpdateScroll` callback that behaves as above.

```jsx
import { ScrollContainer } from 'react-router-scroll';

function Page() {
  /* ... */

  return (
    <ScrollContainer
      scrollKey={scrollKey}
      shouldUpdateScroll={shouldUpdateScroll}
    >
      <MyScrollableComponent />
    </ScrollContainer>
  );
}
```

`<ScrollContainer>` does not support on-the-fly changes to `scrollKey` or to the DOM node for its child.

### Notes

#### Minimizing bundle size

If you are not using `<ScrollContainer>`, you can reduce your bundle size by importing the `useScroll` module directly.

```jsx
import useScroll from 'react-router-scroll/lib/useScroll';
```

#### Server rendering

Do not apply the `useScroll` middleware when rendering on a server. You may use `<ScrollContainer>` in server-rendered components, however, as it will do nothing when rendering on a server.

[build-badge]: https://img.shields.io/travis/taion/react-router-scroll/master.svg
[build]: https://travis-ci.org/taion/react-router-scroll

[npm-badge]: https://img.shields.io/npm/v/react-router-scroll.svg
[npm]: https://www.npmjs.org/package/react-router-scroll
