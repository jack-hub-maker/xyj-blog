## 一、字面量增强

ES6 中对 `对象字面量` 进行了增强，称之为 Enhanced object literals（增强对象字面量）。

字面量的增强主要包括下面几部分：

- 属性的简写：Property Shorthand
- 方法的简写：Method Shorthand
- 计算属性名：Computed Property Names

```js
var name = "tao";
var age = 18;

var obj = {
  // name: name,
  // age: age,
  // foo: function () {
  //   console.log('foo');
  // }
  // 属性简写
  name,
  age,
  // 方法简写
  foo() {
    console.log("foo");
  },
  // 这个可不是字面量增强哦，这个就是一个key为foo，value为箭头函数的属性
  foo: () => {
    console.log(this);
  },
  // 计算属性简写
  [name + 2]: "bar+2",
};

obj[name + 1] = "bar+1";
console.log(obj);
```
