
## 面试题一

```js
var n = 100;

function foo() {
  n = 200;
}

foo();

console.log(n);
```

<details>
<summary>查看解析（点击展开）</summary>

200

```js
/**
 * 按照严格意义的编程逻辑的话，n = 200这本来就是个错误的语法，会直接进行报错的
 * 你首先要定义n，才可以使用n
 * 但是在JavaScript中是一个特殊的语法
 * js引擎在转换的时候，如果你变量没有定义的话，直接赋值的话，它会把这个变量直接放在全局的GO里
 */

/**
 * AO：{}
 * GO：{n:undefined => 100 => 200}
 */
```

</details>

## 面试题二

```js
function foo() {
  console.log(n);
  var n = 200;
  console.log(n);
}

var n = 100;
foo();
```

<details>
<summary>查看解析（点击展开）</summary>

undefined

200

```js
/**
 *  AO：{n:undefined => 200}
 *  GO: {n: undefined => 100}
 */
```

</details>

## 面试题三

```js
var n = 100;

function foo() {
  console.log(n);
}

function bar() {
  var n = 200;
  console.log(n);
  foo();
}

bar();
console.log(n);
```

<details>
<summary>查看解析（点击展开）</summary>

200

100

100

```js
/**
 * foo AO: {}
 * bar AO: {n: undefined => 200}
 * GO: {n: undefined => 100}
 */
```

</details>

## 面试题四

```js
var a = 100;

function foo() {
  console.log(a);
  return;
  var a = 100;
}

foo();
```

<details>
<summary>查看解析（点击展开）</summary>

undefined

```js
/**
 * 这道题还是出错率很高的，很多人都会觉得结果是100，实际上是undefined
 * 为什么？
 * 因为在return语句是执行代码的时候才会执行，解析的时候不会管，所以return下面的变量a依旧是会添加到AO里的
 */

/**
 * AO: {a: undefined}
 * GO: {a: undefined => 100}
 */
```

</details>

## 面试题五

```js
function foo() {
  var a = (b = 100);
}

foo();

console.log(a);
console.log(b);
```

<details>
<summary>查看解析（点击展开）</summary>

a is not defined

b = 100

```js
/**
 * 当定义var a = b = 100这段代码的时候，js引擎会解析成var a = 100，b = 100
 */

/**
 * AO: {a: undefined => 100}
 * GO: {b: undefined => 100}
 */
```

</details>
