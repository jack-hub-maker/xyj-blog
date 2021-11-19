# 手写防抖和节流函数

## 一、自定义防抖函数

### 1.1 基本实现

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null

  // 2.真正执行的函数
  function _debounce() {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer)
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn()
    }, delay);
  }
  // 3.返回函数
  return _debounce
}
```

### 1.2 优化参数和this的执行

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer)
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args)
    }, delay);
  }
  // 3.返回函数
  return _debounce
}
```

### 1.3 优化取消操作（增加取消功能）

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer)
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args)
    }, delay);
  }

  // 封装取消功能（函数也是一个对象，所以我们可以在函数上添加属性
  _debounce.cancel = () => {
    if (timer) clearTimeout(timer)
    timer = null
  }
  // 3.返回函数
  return _debounce
}
```

### 1.4 优化立即执行效果（第一次立即执行）

```js
function debounce(fn, delay, Immediately = false) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null
  let invoke = false

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer)

    // 判断是否立即执行一次
    if (Immediately && !invoke) {
      fn.apply(this, args)
      invoke = true
    }

    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args)
    }, delay);
  }
  _debounce.cancel = () => {
    if (timer) clearTimeout(timer)
    timer = null
  }
  // 3.返回函数
  return _debounce
}
```

### 1.4 优化返回值

```js
function debounce(fn, delay, Immediately = false, resultCallback) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null
  let invoke = false

  // 2.真正执行的函数
  function _debounce(...args) {
    return new Promise((resolve, reject) => {
      // 取消前一次的定时器
      if (timer) clearTimeout(timer)

      // 判断是否立即执行一次
      if (Immediately && !invoke) {
        const result = fn.apply(this, args)
        if (resultCallback) resultCallback(result)
        resolve(result)
        invoke = true
      }

      // 延迟执行
      timer = setTimeout(() => {
        // 外部传入真正执行的函数
        const result = fn.apply(this, args)
        if (resultCallback) resultCallback(result)
        resolve(result)
      }, delay);
    })
  }

  _debounce.cancel = () => {
    if (timer) clearTimeout(timer)
    timer = null
  }

  // 3.返回函数
  return _debounce
}
```