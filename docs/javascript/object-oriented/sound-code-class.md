# 阅读class源码

## 一、ES6 转 ES5 的代码

我们先来看看 class 转为 ES5 是什么样子的

```js
class Person {}
```

转为 ES5

```js
// 判断Person的prototype是否出现在this的原型链上（如果没有出现就抛出异常）
// 这样做其实就是让我们不能直接调用Person()，直接调用里面的this就是window
// 主要是让我们通过new Person的方式来调用,通过new调用,里面的this就是Person的实例了
function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

// 定义了一个Peson函数
// 然后把this和Person传给了_classCallCheck
// 当我们调用Person函数的时候会调用上面的_classCallCheck函数
var Person = function Person() {
  _classCallCheck(this, Person);
};
```

## 二、ES6 转 ES5 的构造方法

我们先来看看 class 里的构造方法和实例方法转为 ES5 是什么样子的

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
function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

// 设置属性描述符
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

## 三、ES6 转 ES5 的继承

我们来看一下 class 继承的代码转换为 ES5 是什么样的

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

class Student extends Person {
  constructor(name, age, friends) {
    super(name, age);
    this.friends = friends;
  }
  eat() {
    console.log(this.name + " eat~");
  }
}
```

我们主要来讲一些主要的逻辑，一些 edge case 感兴趣自己可以看一下

```js
function _typeof(obj) {
  "@babel/helpers - typeof";
  if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") {
    _typeof = function _typeof(obj) {
      return typeof obj;
    };
  } else {
    _typeof = function _typeof(obj) {
      return obj &&
        typeof Symbol === "function" &&
        obj.constructor === Symbol &&
        obj !== Symbol.prototype
        ? "symbol"
        : typeof obj;
    };
  }
  return _typeof(obj);
}

function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: { value: subClass, writable: true, configurable: true },
  });
  if (superClass) _setPrototypeOf(subClass, superClass);
}

function _setPrototypeOf(o, p) {
  _setPrototypeOf =
    Object.setPrototypeOf ||
    function _setPrototypeOf(o, p) {
      o.__proto__ = p;
      return o;
    };
  return _setPrototypeOf(o, p);
}

function _createSuper(Derived) {
  // 判断是否支持ReflectConstruct
  var hasNativeReflectConstruct = _isNativeReflectConstruct();
  return function _createSuperInternal() {
    var Super = _getPrototypeOf(Derived),
      result;
    // 支持
    if (hasNativeReflectConstruct) {
      var NewTarget = _getPrototypeOf(this).constructor;
      /**
       * Super：Person
       * NewTarget：Student
       * Reflect.construct其实会通过第一个参数创建出一个实例，但是这个实例的原型constructor指向的是第三个参数
       * 翻译：创建一个Person实例，实例的类型是Student
       */
      result = Reflect.construct(Super, arguments, NewTarget);
    } else {
      result = Super.apply(this, arguments);
    }
    return _possibleConstructorReturn(this, result);
  };
}

function _possibleConstructorReturn(self, call) {
  if (call && (_typeof(call) === "object" || typeof call === "function")) {
    return call;
  } else if (call !== void 0) {
    throw new TypeError(
      "Derived constructors may only return object or undefined"
    );
  }
  return _assertThisInitialized(self);
}

function _assertThisInitialized(self) {
  if (self === void 0) {
    throw new ReferenceError(
      "this hasn't been initialised - super() hasn't been called"
    );
  }
  return self;
}

function _isNativeReflectConstruct() {
  if (typeof Reflect === "undefined" || !Reflect.construct) return false;
  if (Reflect.construct.sham) return false;
  if (typeof Proxy === "function") return true;
  try {
    Boolean.prototype.valueOf.call(
      Reflect.construct(Boolean, [], function () {})
    );
    return true;
  } catch (e) {
    return false;
  }
}

function _getPrototypeOf(o) {
  _getPrototypeOf = Object.setPrototypeOf
    ? Object.getPrototypeOf
    : function _getPrototypeOf(o) {
        return o.__proto__ || Object.getPrototypeOf(o);
      };
  return _getPrototypeOf(o);
}

function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

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
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}

var Person = /*#__PURE__*/ (function () {
  function Person(name, age) {
    _classCallCheck(this, Person);

    this.name = name;
    this.age = age;
  }

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

var Student = /*#__PURE__*/ (function (_Person) {
  // 这个就跟我们前面讲过的寄生组合式继承是一样的
  _inherits(Student, _Person);

  var _super = _createSuper(Student);

  function Student(name, age, friends) {
    var _this;

    _classCallCheck(this, Student);

    debugger;
    // 这个会发现跟原来我们通过Person.call的方式其实是差不多
    // Person.call(this,naem,age)
    // 但是这个_super它是通过_createSuper创建的
    // 为什么它不直接通过Person的方式进行调用
    // 这是因为前面说过它不允许我们直接调用Person
    _this = _super.call(this, name, age);
    _this.friends = friends;
    return _this;
  }

  _createClass(Student, [
    {
      key: "eat",
      value: function eat() {
        console.log(this.name + " eat~");
      },
    },
  ]);

  return Student;
})(Person);

var s = new Student();
```

总的来说这个源码还是比较难的，目前看不懂的话（第一我表述的还是有问题的，第二里面有很多东西是比较陌生的（需要补）

暂时就这样把，我的基础也不行，目前就写到这里，以后技术提高了，再回头来看看

## 四、阅读源码技巧

阅读源码首先 JS `基础一定要非常好`

- 声明的内容不看
- 不重要的不看
- 一定不要浮躁
- 看到后面忘记前面的东西（vscode 插件：Bookmarks）
- 熟练使用 debugger
