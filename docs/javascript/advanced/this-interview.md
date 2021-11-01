# this指向面试题
## 一、面试题一

```js
var name = "window";
var person = {
  name: "person",
  sayName: function () {
    console.log(this.name);
  }
};
function sayName() {
  var sss = person.sayName;
  sss(); 
  person.sayName(); 
  (person.sayName)(); 
  (b = person.sayName)(); 
}
sayName();
```

<details>
<summary>查看解析（点击展开）</summary>

sss(): window(直接调用函数，默认绑定)

person.sayName(): person(隐式绑定)

(person.sayName)(): person(默认绑定和隐式绑定冲突，隐式绑定优先)

(b = person.sayName)(): window(间接函数绑定)

</details>

## 二、面试题二

```js
var name = 'window'
var person1 = {
  name: 'person1',
  foo1: function () {
    console.log(this.name)
  },
  foo2: () => console.log(this.name),
  foo3: function () {
    return function () {
      console.log(this.name)
    }
  },
  foo4: function () {
    return () => {
      console.log(this.name)
    }
  }
}

var person2 = { name: 'person2' }

person1.foo1(); 
person1.foo1.call(person2); 

person1.foo2();
person1.foo2.call(person2);

person1.foo3()();
person1.foo3.call(person2)();
person1.foo3().call(person2);

person1.foo4()();
person1.foo4.call(person2)();
person1.foo4().call(person2);
```

<details>
<summary>查看解析（点击展开）</summary>

person1.foo1(): person1(隐式绑定)

person1.foo1.call(person2): person2(隐式绑定和显示绑定冲突，显示绑定优先)

person1.foo2(): window(箭头函数不绑定this，找到上层this：window)

person1.foo2.call(person2): window(箭头函数不绑定this，找到上层this：window)

person1.foo3()(): window(直接调用函数，默认绑定)

person1.foo3.call(person2)()：window(绑定了foo3的this为person2，但是跟内部闭包没有关系，还是直接调用函数，默认绑定)

person1.foo3().call(person2)：person2(调用foo3函数返回闭包，然后给闭包显式绑定person2)

person1.foo4()()：person1(直接调用函数，但是闭包是一个箭头函数，箭头函数不绑定this，于是去上层找this，上层foo4的this是通过隐式绑定的person1)

person1.foo4.call(person2)()：perosn2(给foo4显式绑定了person2，但是跟内部闭包没有关系，接着直接调用，内部是一个闭包，闭包是一个箭头函数，箭头函数不绑定this，于是去上层找this，上层foo4的this是person2)

person1.foo4().call(person2)：person1(返回了闭包，然后给闭包显式绑定了person2，但是闭包是一个箭头函数，箭头函数不绑定this)
</details>

## 三、面试题三

```js
var name = 'window'
function Person(name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  },
    this.foo2 = () => console.log(this.name),
    this.foo3 = function () {
      return function () {
        console.log(this.name)
      }
    },
    this.foo4 = function () {
      return () => {
        console.log(this.name)
      }
    }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.foo1()
person1.foo1.call(person2)

person1.foo2()
person1.foo2.call(person2)

person1.foo3()()
person1.foo3.call(person2)()
person1.foo3().call(person2)

person1.foo4()()
person1.foo4.call(person2)()
person1.foo4().call(person2)
```

<details>
<summary>查看解析（点击展开）</summary>

person1.foo1()：person1(隐式绑定)

person1.foo1.call(person2)：person2(显式绑定)

person1.foo2()：person1(隐式绑定，但是foo2是一个箭头函数，箭头函数不绑定this，上层this是person1对象)

person1.foo2.call(person2)：person1(显式绑定，但是foo2是一个箭头函数，箭头函数不绑定this，上层this是person1对象)

person1.foo3()()：window(默认绑定)

person1.foo3.call(person2)()：window(显式绑定foo3的this为person2，但是跟里面的函数没有关系，直接调用的函数默认绑定)

person1.foo3().call(person2)：person2(显式绑定里面的函数为person2)

person1.foo4()()：person1(默认绑定，但是里面的函数是一个箭头函数，箭头函数不绑定this，上层this是person1对象)

person1.foo4.call(person2)()：person2(显式绑定foo4函数的this为person2，但是跟里面的函数没有关系，默认绑定里面的函数，但是里面的函数是一个箭头函数，箭头函数不绑定this，上层this是foo4，foo4的this是person2)

person1.foo4().call(person2)：person1(显式绑定里面的函数为person2，但是里面的函数是一个箭头函数，箭头函数不绑定this，上层this是person1对象)

</details>

## 四、面试题四

```js
var name = 'window'
function Person (name) {
  this.name = name
  this.obj = {
    name: 'obj',
    foo1: function () {
      return function () {
        console.log(this.name)
      }
    },
    foo2: function () {
      return () => {
        console.log(this.name)
      }
    }
  }
}
var person1 = new Person('person1')
var person2 = new Person('person2')

person1.obj.foo1()()
person1.obj.foo1.call(person2)()
person1.obj.foo1().call(person2)

person1.obj.foo2()()
person1.obj.foo2.call(person2)()
person1.obj.foo2().call(person2)
```

<details>
<summary>查看解析（点击展开）</summary>

person1.obj.foo1()()：window(直接调用函数，默认绑定)

person1.obj.foo1.call(person2)()：window(显式绑定foo1位person2，但是跟里面的函数没有关系，直接调用里面的函数默认绑定)

person1.obj.foo1().call(person2)：person2(显式绑定里面的this为person2)

person1.obj.foo2()()：obj(默认绑定，但是里面函数是一个箭头函数，箭头函数不绑定this，向上层找this，上层是obj隐式调用了foo2，所以上层this是obj)

person1.obj.foo2.call(person2)()：person2(显式绑定foo2函数的this为person2，直接调用内部函数，但是内部函数是一个箭头函数，箭头函数不绑定this，上层this是foo2)

person1.obj.foo2().call(person2)：obj(显式绑定内部函数this为person2，但是内部函数是一个箭头函数，箭头函数不绑定this，上层this是foo2，foo2的this是obj隐式绑定的this)

</details>