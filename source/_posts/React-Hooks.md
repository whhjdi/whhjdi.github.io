---
title: React-Hooks
date: 2019-10-01 17:31:19
tags: [react,hooks]
cover_img:
feature_img:
description:
keywords:
---

# React Hooks

## State hooks

useState 是一个函数通过调用它，并给他一个默认值就可以创建一个 state,有了 hooks 就赋予了函数组件维护自己状态的能力，并且改变状态再也不需要 this.setState 了，也没有了忘记绑定 this 的困扰

```jsx
import React, { useState } from "react";

function FirstExample() {
  const [count, setCount] = useState(0);

  return (
    <div>
      count is {count}
      <button onClick={() => setCount(count + 1)}>count加1</button>
    </div>
  );
}

export default FirstExample;
```

## Effect Hooks

useEffect 是一个非常重要的 hook,可以让你在函数组件中执行副作用操作

什么是副作用呢？ 数据获取，设置订阅以及手动更改 React 组件中的 DOM 都属于副作用

在以前我们经常在 componentDidMount 和 componentDidUpdate 中执行相同的逻辑，并且在 componentWillUnmount 中清除某些东西，useEffect 就是用来替代这三个生命周期函数的

useEffect 会在每次重新渲染时执行，如果第二个参数是一个空数组，那么他将会只执行一次。第二个参数表示她依赖的值，这个值改变时才会执行

useEffect 的对调函数支持返回一个清除函数，它会在组件卸载时执行清除操作，当不需要清除副作用时就不用返回

并且在执行当前 effect 时会对上一个 effect 进行清除

官方建议使用[`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation)

来发出一些警告，避免你添加错误的依赖，并给出修复的建议

```jsx
import React, { useState, useEffect } from "react";

function FirstExample() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `你点击了${count}次`;
  }, [count]);
  return (
    <div>
      count is {count}{" "}
      <button onClick={() => setCount(count + 1)}> count加1 </button>{" "}
    </div>
  );
}

export default FirstExample;
```

## 自定义 hook

自定义 hooks 其实就是一个函数，只不过这个函数命名使用 use 开头（约定）

通过自定义 hook 我们可以把一些逻辑提取到可重用的函数中，这也是 hooks 的一个好处

你可以多次调用自定义的 hook,多次调用的他们的 state 是完全独立的，不会相互影响

举个例子，我们完全可以把上面修改 title 的方法提取出来变成一个自定义的 hook

```jsx
import { useEffect } from "react";

function useTitle(title) {
  useEffect(() => {
    document.title = title;
  }, [title]);
}
export default useTitle;
```

```jsx
import React, { useState } from "react";
import useTitle from "./useTitle";
function FirstExample() {
  const [count, setCount] = useState(0);
  useTitle(count);
  return (
    <div>
      count is {count}{" "}
      <button onClick={() => setCount(count + 1)}> count加1 </button>{" "}
    </div>
  );
}

export default FirstExample;
```

## useContext

```jsx
const countContext = createContext();
function App() {
  const [count, setCount] = useState(1);
  return (
    <countContext.Provider value={count}>
      <div>
        <Foo></Foo>
      </div>
    </countContext.Provider>
  );
}

function Foo() {
  const count = useContext(countContext);
  return <div>{count}</div>;
}

export default App;
```

## useMemo

```jsx
function App() {
  const [count, setCount] = useState(1);
  const double = useMemo(() => {
    return count * 2;
  }, [count === 3]);
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        click
      </button>
      <h1>{double}</h1>
      <h2>{count}</h2>
    </div>
  );
}
```

## useCallback

```jsx
useCallback(fn, deps) 相当于 useMemo(() => fn, deps)
```

## 自定义 hook

```jsx
function App() {
  const size = useSize();
  console.log(size);
  return (
    <div>
      {size.width}x{size.height}
    </div>
  );
}

function useSize() {
  const [size, setSize] = useState({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
  });

  const onResize = useCallback(() => {
    setSize({
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    });
  }, []);
  useEffect(() => {
    window.addEventListener("resize", onResize);
    return () => {
      window.addEventListener("resize", onResize);
    };
  });
  return size;
}
export default App;
```
