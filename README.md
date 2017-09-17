# EventEmitter

## 介绍

一个简单的 EventEmitter，可在浏览器中使用，帮助你实现事件的订阅和发布。

## 依赖

原生 JavaScipt 实现，无依赖。

## 大小

压缩后 1KB，gzip 压缩后更小。

## 使用

```html
<script src="path/eventEmitter.js"></script>
```

或者

```js
import eventEmitter from 'path/eventEmitter.js'
```

## API

```js
var emitter = new EventEmitter();
```

### on

添加一个事件监听器，支持链式调用

```js
emitter.on(eventName, listener)
```

* eventName 事件名称
* listener 监听器函数

### off

删除一个事件监听器，支持链式调用

```js
emitter.on(eventName, listener)
```

* eventName 事件名称
* listener 监听器函数

### once

添加一个只能触发一次的事件监听器，支持链式调用

```js
emitter.once(eventName, listener)
```

* eventName 事件名称
* listener 监听器函数

### emit

触发事件，支持链式调用

```js
emitter.emit(eventName, args)
```

* eventName 事件名称
* arg 数组形式，传入事件监听器的参数

### allOff

删除某个事件或者所有事件

```js
emitter.allOff(eventName)
```

* eventName 事件名称 如果不传，则删除所有事件

## 示例代码

### 添加、触发、删除事件

```js
var emitter = new EventEmitter();

function handleOne(a, b, c) {
    console.log('第一个监听函数', a, b, c)
}

function handleSecond(a, b, c) {
    console.log('第二个监听函数', a, b, c)
}

function handleThird(a, b, c) {
    console.log('第三个监听函数', a, b, c)
}

emitter.on("demo", handleOne)
       .once("demo", handleSecond)
       .on("demo", handleThird);

emitter.emit('demo', [1, 2, 3]);
// => 第一个监听函数 1 2 3
// => 第二个监听函数 1 2 3
// => 第三个监听函数 1 2 3

emitter.off('demo', handleThird);
emitter.emit('demo', [1, 2, 3]);
// => 第一个监听函数 1 2 3

emitter.allOff();
emitter.emit('demo', [1, 2, 3]);
// nothing
```

### 支持在监听器函数中删除未执行的某个事件

```js
var emitter = new EventEmitter();

function handleOne(a, b, c) {
    console.log('第一个监听函数', a, b, c)
    emitter.off("demo", handleSecond)
}

function handleSecond(a, b, c) {
    console.log('第二个监听函数', a, b, c)
}

function handleThird(a, b, c) {
    console.log('第三个监听函数', a, b, c)
}

emitter.on("demo", handleOne)
       .on("demo", handleSecond)
       .on("demo", handleThird);

emitter.emit('demo', [1, 2, 3]);

// => 第一个监听函数 1 2 3
// => 第三个监听函数 1 2 3
```
