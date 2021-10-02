## ES6 转 ES5 的代码

我们来看一下这段代码

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  say() {
    console.log(this.name + " say~");
  }
}
```

看一下转成 ES5 是什么样子

```js
// 做了一些edge case的处理
function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

// 这个就跟前面的学过了寄生组合式继承的思路是一样的
function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i];
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if ("value" in descriptor) descriptor.writable = true;
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  // 判断第二个属性有没有值,有值的话就会把Person的原型和值传到_defineProperties函数里
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  // 判断是否有静态方法,如果有就直接把静态方法定义到Person上面
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}

// 它把我们的Perosn构造函数放在一个立即执行函数里面
// 立即执行函数（是为了防止命名冲突）
// /*#__PURE__*/（标记）魔法注释，这里是把这个Person函数标记成是一个纯函数
// 纯函数在webpack压缩的时候有一个tree-shaking（https://webpack.js.org/guides/tree-shaking/）（性能优化）
var Person = /*#__PURE__*/ (function () {
  function Person(name, age) {
    _classCallCheck(this, Person);

    this.name = name;
    this.age = age;
  }
  // 我们如果自己往原型上加方法的是通过prototype.xx的方法添加
  // 这里调了一个_createClass,主要是为了代码的复用
  // 接着我们上去看这个_createClass函数
  _createClass(Person, [
    {
      key: "say",
      value: function say() {
        console.log(this.name + " say~");
      },
    },
  ]);

  return Person;
})();
```

## ES6 转 ES5 的继承
