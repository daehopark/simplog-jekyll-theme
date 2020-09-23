---
layout: post
title: "[Web] Sync Async Blocking Non-Blocking"
tags: ["web"]
comments: true
---

`Sync`, `Async`, `Blocking`, `Non-Blocking`의 차이점은 무엇일까? `Sync`와 `Async`의 관심사는 흐름에 있다면 `Blocking`과 `Non-Blocking`의 관심사는 제어권에 있다고 할 수 있다. 즉 앞의 2개와 뒤의 2개의 관심사가 다르기 때문에 독립적으로 봐야한다.

## Sync vs Async

위에서 말했듯이 `Sync`와 `Async`의 관심사는 흐름에 있다. `Sync`는 코드가 순차적으로 실행되는 특징을 가지며 `Async`는 코드의 순서가 보장되지 않는다. `Sync`의 예제를 보자.

```js
const fs = require("fs");

const taskA = () => {
  console.log("Task A");
  const data = fs.readFileSync("hello.txt", "utf-8");
  console.log(data);
};

const taskB = () => {
  console.log("Task B");
};

taskA();
taskB();
```

```
Task A
Hello World!
Task B
```

`TaskA`가 끝날 때까지 `TaskB`는 실행될 수 없다. `Async`의 예제를 보자.

```js
const fs = require("fs");

const taskA = () => {
  console.log("Task A");
  fs.readFile("hello.txt", "utf-8", (error, data) => {
    console.log(data);
  });
};

const taskB = () => {
  console.log("Task B");
};

taskA();
taskB();
```

```
Task A
Task B
Hello World!
```

`TaskA`가 끝나지 않더라도 `TaskB`를 먼저 실행할 수 있다.

## Blocking vs Non-Blocking

`Blocking`과 `Non-Blocking`의 관심사는 제어권에 있다. 하나의 시스템이 다른 시스템을 호출할 때 다른 시스템이 끝날 때까지 제어권을 반납하지 않으면 `Blocking`이 되고 제어권을 반납하면 `Non-Blocking`이 된다.

## References

- [Blocking-NonBlocking-Synchronous-Asynchronous](https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)
- [동기/비동기와 블로킹/논블로킹](https://deveric.tistory.com/99)
