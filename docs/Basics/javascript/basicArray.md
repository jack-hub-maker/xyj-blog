# 数组和对象

## **关于数组**

一些关于js数组的基础知识点，只是日常使用，不攻坚。如有什么错误，欢迎大家指正😄

#### Array.prototype.map()

返回一个新的Array，每个元素为调用func的结果。新数组的长度和原来的是一样的，他只不过是逐一对原来数据里的每个元素进行操作，并且返回函数处理后的结果。 
语法： var newArray = arr.map(function callback(currentValue, index, array){ //对每个元素的处理 }) 
参数 callback:用来生成新数组用的函数。 callback的参数： currentValue：当然正在处理的元素 index:正在处理元素的索引 array:调用map方法的数组（就是.map()前面的也就是arr）

```javascript
var a = [1,2,3,4];
var newa = a.map(function(x){
 return x > 1;
});
console.log(newa,a); 
//newa : [false, true, true, true]    //a: 1 2 3 4

var b = [1,2,3,4];
var newb = a.map((item)=>{
    return item+1
});
console.log(newb,b); //newb : [2, 3, 4, 5] //b: [1, 2, 3, 4]
```



#### Array.prototype.filter() 

创建一个新数组，其结果是调用一个函数过滤后的所有符合条件的元素。 筛选条件，把数组符合条件的放在新的数组里面返回。新数组和原来的数组长度不一定一样。注意当对象数组使用filter时找到的是所有符合条件的item的数组
 语法 var newArray = arr.filter(function callback(curValue, index , array){ //函数代码 }); 
 参数 callback: 调用的过滤函数 curValue： 当前元素 index： 当前元素的索引 array：调用filter的数组

```javascript
var a = [1,2,3,4];
var newa = a.filter(function(x){
 return x > 1;
});
console.log(newa,a); 
//newa : 2 3 4    //a: 1 2 3 4
```

 基本用法就是以上那些 因为都是对元素的遍历，所以我猜想 如果把map中调用的函数换成filter里面的过滤函数是否能得到相同的结果呢。 于是

```js
var a = [1,2,3,4];
var newa = a.map(function(x){
 return x > 1;
});
console.log(newa,a); 
//newa :false true true true     //a: 1 2 3 4
```

 可以看出来newa 的到的并不是数字，它们只是对当前元素调用函数后（x是否大于1）的结果。而filter 会将结果为true的数组存到新的数组里面。 



#### Array.prototype.find()

find 查找 返回第一个符合条件的结果,注意返回的是符合条件的item对象。

```js
var data= [{name:"Jackie",id: "122"}, 
           {name:"Tony2",id: "121"}, 
           {name:"Tony",id: "121"}];
console.log(data.find(user=>user.id==='121')) //{name: "Tony2", id: "121"}
```

#### Array.prototype.findIndex()

findIndex() 方法返回传入一个测试条件（函数）符合条件的数组第一个元素位置。

- 如果没有符合条件的元素返回 -1
- findIndex() 对于空数组，函数是不会执行的
- findIndex() 并没有改变数组的原始值

```js
var data= [{name:"Jackie",id: "122"}, 
           {name:"Tony2",id: "121"}, 
           {name:"Tony",id: "121"}];
console.log(data.findIndex(user=>user.id==='121')) //1
```



#### Array.prototype.from()

from() 方法用于通过拥有 length 属性的对象或可迭代的对象来返回一个数组。

如果对象是数组返回 true，否则返回 false。

```js
var myArr = Array.from("RUNOOB");//通过字符串创建一个数组：
console.log(myArr)// ["R", "U", "N", "O", "O", "B"]

var setObj = new Set(["a", "b", "c"]);
var objArr = Array.from(setObj);
console.log(objArr)//["a", "b", "c"]
```



#### Array.prototype.includes()

includes() 方法用来判断一个数组是否包含一个指定的值，如果是返回 true，否则false。

```js
let site = ['runoob', 'google', 'taobao']; 
site.includes('runoob');  // true  
site.includes('baidu');  // false
```

#### Array.prototype.indexOf()

indexOf() 方法可返回数组中某个指定的元素位置。

该方法将从头到尾地检索数组，看它是否含有对应的元素。开始检索的位置在数组 start 处或数组的开头（没有指定 start 参数时）。如果找到一个 item，则返回 item 的第一次出现的位置。开始位置的索引为 0。

如果在数组中没找到指定元素则返回 -1。

**提示**如果你想查找字符串最后出现的位置，请使用 [lastIndexOf() 方法](https://www.runoob.com/jsref/jsref-lastindexof-array.html)。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");//2
```



#### Array.prototype.join()

join() 方法用于把数组中的所有元素转换一个字符串。

元素是通过指定的分隔符进行分隔的。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var energy = fruits.join();//Banana,Orange,Apple,Mango

var fruits = ["Banana", "Orange", "Apple", "Mango"];
var energy = fruits.join(" and ");//Banana and Orange and Apple and Mango
```



#### Array.prototype.pop()

pop() 方法用于删除数组的最后一个元素并返回删除的元素。

**注意：**此方法改变数组的长度！

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.pop();//Banana,Orange,Apple
```

#### Array.prototype.shift()

shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。

**注意：** 此方法改变数组的长度！

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift()//Orange,Apple,Mango
```

#### Array.prototype.push()

push() 方法可向数组的末尾添加一个或多个元素，并返回新的长度。

**注意：** 新元素将添加在数组的末尾。

**注意：** 此方法改变数组的长度。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi")//Banana,Orange,Apple,Mango,Kiwi
```

#### Array.prototype.unshift()

unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。

**注意：** 新元素将添加在数组的开头

**注意：** 该方法将改变数组的数目

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift("Kiwi")//Kiwi,Banana,Orange,Apple,Mango
```



#### Array.prototype.slice()

slice() 方法可从已有的数组中返回选定的元素。

slice()方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。

**注意：** slice() 方法不会改变原始数组。

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1,3);//Orange,Lemon
```



#### Array.prototype.splice()

splice() 方法用于添加或删除数组中的元素。

**注意：**这种方法会改变原始数组。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0,1);//Orange,Apple,Mango
fruits.splice(2,0,"Lemon","Kiwi");//Banana,Orange,Lemon,Kiwi,Apple,Mango
```



#### Array.prototype.toString()

toString() 方法可把数组转换为字符串，并返回结果。

**注意：** 数组中的元素之间用逗号分隔。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.toString();//Banana,Orange,Apple,Mango
```



#### Array.prototype.sort()

sort 按指定条件排序

排序顺序可以是字母或数字，并按升序或降序。

默认排序顺序为按字母升序。

```js
var a = [1,2,3,4];
console.log(a.sort((a,b)=> a - b))//1234正向排序
console.log(a.sort((a,b)=> b - a))//4321从大到小排序

var points = [40,100,1,5,25,10];
points.sort(function(a,b){return a-b});//1,5,10,25,40,100

var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();//Apple,Banana,Mango,Orange
fruits.reverse();//Orange,Mango,Banana,Apple

var data= [{name:"Jackie",id: "122"}, 
           {name:"Tony2",id: "121"}, 
           {name:"Tony",id: "120"}];
var sums = data.sort((a, b) => (a.id > b.id) ? 1 : ((b.id > a.id) ? -1 : 0))
console.log(sums)
//根据id从小到大排序：排序结果
(3) [{…}, {…}, {…}]
0: {name: "Tony", id: "120"}
1: {name: "Tony2", id: "121"}
2: {name: "Jackie", id: "122"}
length: 3
__proto__: Array(0)
```

#### Array.prototype.reverse() 

reverse() 方法反转数组中元素的顺序。

**注释：**reverse() 方法将改变原始数组。可用JSON.parse(JSON.stringify(fruits))深拷贝一份数据

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.reverse())//["Mango", "Apple", "Orange", "Banana"]
console.log(fruits)//["Mango", "Apple", "Orange", "Banana"]原数组被改变
```

#### Array.prototype.concat() 

concat() 方法用于连接两个或多个数组。

该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本。

```js
var a = ["Cecilie", "Lone"]; 
var b = ["Emil", "Tobias", "Linus"]; 
var c = ["Robin"]; 
var children = a.concat(b,c);
console.log(children)//Cecilie,Lone,Emil,Tobias,Linus,Robin
var ES6children = [...a,...b,...c]//推荐使用ES6扩展字符串更好的处理合并数组
```



#### Array.prototype.forEach() 

数组的每一个元素执行一次提供的函数。

```js
var a = [1,2,3,4];
var newa = a.forEach(function(x){
 return x > 1;
});
console.log(newa,a);  //undifined  //1 2 3 4 
var a = [1,2,3,4];
var newa = a.forEach(function(x){
 console.log(x);
});
console.log(newa,a);  
//1
//2
//3
//4
//undifined  //1 2 3 4 
```

 从上面可以看出forEach 只是让数组里面的元素执行一次函数，并不会对原数组产生影响，也不会获得新的数组 

####  Array.prototype.reduce() 

接受一个函数作为累加器，依次加上数组的当前元素。 
 语法 arr.reduce(callback(previousValue, currentValue, currentIndex,array),initialValue); 
 参数 callback:累加器函数 previousValue: 上一次调用函数后的返回值，或者是提供的初始值（initialValue） currentValue:当前数组的元素 currentIndex:当前数组元素的索引 array:调用reduce的数组 initialValue:初始值，作为第一次调用callback的第一个参数，也可不写，默认为0；

```js
var a = [1,2,3,4];
var new = a.reduce(function(total, current){
 return total + current;
},100);
console.log(new,a);
//10   //1 2 3 4
```

**关于reduce的作用**

我们可以用reduce方法做些什么呢？

**1.数组求和**

reduce方法本意就是用来记录循环的累积结果，用于数组求和是最合适不过了。比如我们要求数组 [1,2,3,4] 的元素之和，用forEach你得这样写：

```
let total = 0;
[1, 2, 3, 4].forEach(item => total += item);
console.log(total); //10
```

但通过reduce方法就简单的多，我们可以这么写：

```
let total = [1, 2, 3, 4].reduce((accumulator, current) => accumulator += current); // 10
```

假设我们希望求数字90与数组 [ 1,2,3,4] 元素的和呢，那就这么写：

```
let total = [1, 2, 3, 4].reduce((accumulator, current) => accumulator += current, 90); // 100
```

**2.数组去重**

比如我们要将数组 [1,2,2,4,null,null] 去除掉重复项，用filter可以这样做：

```
let arr = [1, 2, 2, 4, null, null].filter((item, index, arr) => arr.indexOf(item) === index); // [1,2,4,null]
```

当然单说实现使用 new Set 更简单：

```js
let arr = [...new Set([1, 2, 2, 4, null, null])]; // [1,2,4,null]
```

现在我们知道了reduce方法，其实也可以通过reduce去重，像这样：

```js
let arr = [1, 2, 2, 4, null, null].reduce((accumulator, current) => {
    return accumulator.includes(current) ? accumulator : accumulator.concat(current);
}, []);
```

**3.数组降维**

比如我们要将二维数组 [[1,2],[3,4],[5,6]] 降维成一维数组，最简单的做法是通过flat方法，像这样：

```js
let arr = [[1,2],[3,4],[5,6]].flat();//[1, 2, 3, 4, 5, 6]
```

通过reduce也挺简单，我们可以结合concat方法，像这样：

```js
let arr = [[1,2],[3,4],[5,6]].reduce((accumulator, current)=>accumulator.concat(current),[]);//[1, 2, 3, 4, 5, 6]
```

那如果是个多维数组呢，reduce可以这样做：

```js
let arr = [0,[1],[2, 3],[4, [5, 6, 7]]];

let dimensionReduction = function (arr) {
    return arr.reduce((accumulator, current) => {
        return accumulator.concat(
            Array.isArray(current) ? 
            dimensionReduction(current) : 
            current
            );
    }, []);
}
dimensionReduction(arr); //[0, 1, 2, 3, 4, 5, 6, 7]
```

相对而言，多维数组降维flat会更简单，当然flat存在兼容问题：

```js
let arr = [0,[1],[2, 3],[4, [5, 6, 7]]].flat(Infinity);// [0, 1, 2, 3, 4, 5, 6, 7]
```

 **使用注意**

在使用reduce方法有一点需要注意，若有初始值但数组为空，或无初始值但数组只有一项时，reduce方法都不会执行。

```js
[].reduce(() => console.log(1), 1); //不会执行
[1].reduce(() => console.log(1)); //不执行
```

若数组为空且没有初始值，reduce方法报错。

```js
[].reduce(() => console.log(1)); //报错
```

所以如果没有初始值，你至少得保证数组有2项才能执行；如果给了初始值，你至少得保证数组有一项才能执行。

```js
[1, 2].reduce(() => console.log(1)); //1
[1].reduce(() => console.log(1), 1); //1
```

#### Array.prototype.some()

返回一个boolean，判断是否有元素是否符合func条件。数组里面所有的元素有一个符合条件就返回true。

#### Array.prototype.every()

返回一个boolean，判断每个元素是否符合func条件。数组里面所有的元素都符合才返回true。

```jsx
let list = [
    {name:"aaa",age:3},
    {name:"bbb",age:4},
    {name:"ccc",age:5},
];
 var eve= list.every(function(item){
   return item.age > 4
})
console.log(eve)//false;
var some = list.some(function(item){
   return item.age > 4
})
console.log(some)//true
```



#### 原始for循环

```js
var arr = [1,2,3,4]
for(var i = 0 ; i< arr.length ; i++){
	console.log(arr[i])
}
1234
```

for循环中可以使用return、break等来中断循环



#### for…in

循环遍历的值是数据结构的键名

```js
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o in obj) {
    console.log(o)    //遍历的实际上是对象的属性名称 a,b,c,d
    console.log(obj[o])  //这个才是属性对应的值1，2，3，4
}
```

总结一句: for in也可以循环数组但是特别适合遍历对象



#### for…of

它是ES6中新增加的语法，用来循环获取一对键值对中的值

```js
循环一个数组
let arr = ['China', 'America', 'Korea']
for (let o of arr) {
    console.log(o) //China, America, Korea
}
```

循环一个普通对象（报错）

```js
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o of obj) {
    console.log(o)   //Uncaught TypeError: obj[Symbol.iterator] is not a function
}
```

一个数据结构只有部署了 Symbol.iterator 属性, 才具有 iterator接口可以使用 for of循环。例子中的obj对象没有Symbol.iterator属性 所以会报错。

哪些数据结构部署了 Symbol.iteratoer属性了呢?

数组 Array
Map
Set
String
arguments对象
Nodelist对象, 就是获取的dom列表集合
如果想让对象可以使用 for of循环怎么办?使用 Object.keys() 获取对象的 key值集合后,再使用 for of

```js
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o of Object.keys(obj)) {
    console.log(o) // a,b,c,d
}
```

或者使用内置的Object.values()方法获取对象的value值集合再使用for of

```js
let obj = {a: '1', b: '2', c: '3', d: '4'}
for (let o of Object.values(obj)) {
    console.log(o) // 1,2,3,4
}
```

循环一个字符串

```js
let str = 'love'
for (let o of str) {
    console.log(o) // l,o,v,e
}
```

循环一个Map

```js
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);

for (let [key, value] of iterable) {
  console.log(value);
}
// 1
// 2
// 3

for (let entry of iterable) {
  console.log(entry);
}
// [a, 1]
// [b, 2]
// [c, 3]
```

循环一个Set

```js
let iterable = new Set([1, 1, 2, 2, 3, 3]);

for (let value of iterable) {
  console.log(value);
}
// 1
// 2
// 3
```



## 关于对象Object

#### 1. Object.assign()

Object.assign() 用于将所有可枚举属性的值从一个或多个源对象，复制到目标对象。

语法：**Object.assign(obj, ...sources)** 

- - obj：目标对象
  - sources：源对象，可以是多个
  - 返回目标对象

**复制一个对象**

```js
const obj = { a: 1 }
const copy = Object.assign({}, obj)
console.log(copy); // { a: 1 }
```

**合并对象**

```js
const obj1 = { a: 1, b: 2 }
const obj2 = { b: 3, c: 4 }
const obj3 = { c: 5, d: 6 }

const obj = Object.assign(obj1, obj2, obj3)
console.log(obj)  // {a: 1, b: 3, c: 5, d: 6}
```

注：如果目标对象与源对象有同名属性，则后面的属性会覆盖前面的属性；如果多个源对象有同名的属性，则后面的源对象会覆盖前面的。

**对象的深拷贝**

通过 Object.assign() 我们可以很快的实现对象的复制，然而这只是一个**浅拷贝**，因为 Object.assign() 方法拷贝的是可枚举属性值，如果源对象的属性值是一个对象的引用，那么该方法也只会复制其引用。

```js
// 浅拷贝
let o1 = {
    a: 0,
    b: { c: 0 }
}
const o2 = Object.assign({}, o1)
o1.b.c = 1
console.log(o2.c) // 1
```

那么，我们怎么实现一个对象的**深拷贝**呢？

如果不考虑保留内置类型，最快速的方法是**通过JSON.stringify() 将该对象转换为 json 字符串表现形式，然后再将其解析回对象。**

```js
// 深拷贝：方法一
let obj1 = { a: 0 , b: { c: 0}}
let obj2 = JSON.parse(JSON.stringify(obj1))
obj1.a = 4
obj1.b.c = 4
console.log(obj1) // { a: 4, b: { c: 4}} 
console.log(obj2) // { a: 0, b: { c: 0}}
```

还可以通过**递归**的方法实现深拷贝

```js
// 深拷贝：方法二
const obj = {
    name: 'andy',
    age: 18,
    info: {
        gender: 'male',
        schoole: 'NEWS'
    },
    color: ['pink', 'yellow']
}
function deepCopy(obj){
    let o = {}
    for(let k in obj) {  //  此处的for...in方法可以使用Object.keys(obj).map(k =>{....}) 替换
        // 1.获取属性值
        var item = obj[k];
        // 2.判断属性值属于哪种数据类型
        if(item instanceof Array) { // 是否是数组
            o[k] = []
            deepCopy(o[k], item)
        } else if(item instanceof Object) { // 是否是对象
            o[k] = {}
            deepCopy(o[k], item)
        } else { // 简单数据类型
            o[k] = item
        }
    }
    return o
}
const newObj = deepCopy(obj)
console.log(newObj)
```

 其实还可以用lodash.cloneDeep来实现

```jsx
import _ from 'lodash'
var obj = {id:1,name:{a:'xx'},fn:function(){}};
var obj2 = _.cloneDeep(obj);
obj2.name.a = 'obj2';
console.log(obj,obj2)
```

```js
var objects = [{ 'a': 1 }, { 'b': 2 }];
var deep = _.cloneDeep(objects);//此处为深拷贝
console.log(deep[0] === objects[0]);
// => false
```



```js
var objects = [{ 'a': 1 }, { 'b': 2 }];
var shallow = _.clone(objects);//此处为浅拷贝
console.log(shallow[0] === objects[0]);
// => true
```



#### 2. Object.create() 

Object.create() 创建一个新对象

语法：**Object.create(proto)**

- - proto：新创建对象的原型对象
  - 返回一个新对象

使用：

```js
const person = {
    isHuman: false,
    printIntroduction: function() {
        console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
    }
}
const me = Object.create(person)
console.log(me) // {}
me.name = 'Matthew' // "name" is a property set on "me", but not on "person"
me.isHuman = true // inherited properties can be overwritten
me.printIntroduction() // My name is Matthew. Am I human? true
console.log(me.__proto__ === person) //true

// 通过new Object()方式创建对象
const me = new Object(person)
console.log(me) // {isHuman: false, printIntroduction: ƒ}
me.name = 'Matthew'
me.isHuman = true
console.log(me) // {isHuman: true, name: "Matthew", printIntroduction: ƒ}
```

Object.create() 可以实现对象的继承，可以通过对象实例的 __proto__ 属性访问原型链上的属性。

new Object() 方法创建的对象，给对象原型添加属性和方法则需要通过构造函数或者类。

```js
function Person(name, age){
    this.name=name
    this.age=age
}
Person.prototype.sayHi = function(){
    console.log(this.name)
}
let p = new Person('jack', 20)
console.log(p.sayHi()) // jack
console.log(p.__proto__ === Person.prototype) // true
```

 

#### 3. Object.defineProperty()

Object.defineProperty() 添加或修改现有属性，并返回该对象。

语法：**Object.defineProperty(obj, prop, description)**

- - obj：必需。目标对象
  - prop：必需。需定义或修改的属性的名字
  - descriptor：必需。目标属性所拥有的特性，以对象形式｛｝书写。

  1. 1. value：设置属性的值，默认为 undefined
     2. writable：值是否可以重写，true | false，默认为 false
     3. enumerable：目标属性是否可以被枚举，true | false，默认为 false
     4. configurable：目标属性是否可以被删除或是否可以再次修改特性，true | false，默认为 false

```js
const obj = {
    id: 1,
    name: 'phone',
    price: '$599'
}
Object.defineProperty(obj, 'num', {
    value: 100,
    enumerable: false
})
console.log(obj) // {id: 1, name: "phone", price: "$599", num: 100}
console.log(Object.keys(obj)) // ["id", "name", "price"]
```

 

#### 4. Object.entires()

Object.entires() 遍历并返回该对象可枚举属性的键值对数组

具体可参考：https://blog.csdn.net/qq_42628504/article/details/107739619

语法：**Object.entires(obj)**

- - 返回对象可枚举属性的键值对数组

```js
const obj = {
    a: 'apple',
    b: 'bar'
}
console.log(Object.entries(obj)) // [ ['a', 'apple'], ['b', 'bar'] ]
for (const [key, value] of Object.entries(obj)) {
    console.log(`${key}: ${value}`) //"a: somestring"  "b: 42"
}
Object.entries(obj).forEach(([key, value]) => {
    console.log(`${key}: ${value}`) // "a: somestring", "b: 42"
})
```

 

#### 5. Object.keys() 

Object.keys() 用于获取对象自身所有可枚举的属性

语法：**Object.keys(obj)**

- - 效果类似于 for...in
  - 返回一个由属性名组成的数组

```js
const obj = {
    id: 1,
    name: 'phone',
    price: '$599',
    num: 100
}
const objKeys = Object.keys(obj);
console.log(objKeys); //['id', 'name', 'price', 'num']

const arr = ['a', 'b', 'c']
const arrKeys = Object.keys(arr)
console.log(arrKeys) //['0', '1', '2']
```

补充：如果只要获取对象的可枚举属性，可用`Object.keys`或用`for...in`循环（`for...in`循环会得到对象自身的和继承的可枚举属性，可以使用 `hasOwnProperty()`方法过滤掉）

 

#### 6. Object.values() 

Object.values() 获取对象自身所有可枚举的属性值

语法：**Object.values(obj)**

- - 返回一个由属性值组成的数组

```js
const obj = {
    id: 1,
    name: 'phone',
    price: '$599',
    num: 100
}
const objValues = Object.values(obj);
console.log(objValues); //[1, 'phone', '$599', '100']
```

#### 附图：对象的所有属性和方法

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/792843d6986d42888019bf30d540dc6e~tplv-k3u1fbpfcp-zoom-1.image)

## 关于new Set

Set 对象存储的值总是唯一的

#### Set 对象方法

| 方法    | 描述                                                |
| :------ | :-------------------------------------------------- |
| add     | 添加某个值，返回Set对象本身。                       |
| clear   | 删除所有的键/值对，没有返回值。                     |
| delete  | 删除某个键，返回true。如果删除失败，返回false。     |
| forEach | 对每个元素执行指定操作。                            |
| has     | 返回一个布尔值，表示某个键是否在当前 Set 对象之中。 |

#### Set 对象作用

- **数组去重**

```js
var arr = [1,2,3,3,1,4];
[...new Set(arr)]; // [1, 2, 3, 4]
Array.from(new Set(arr)); // [1, 2, 3, 4]
[...new Set('ababbc')].join(''); // "abc" 字符串去重
new Set('ice doughnut'); //Set(11) {"i", "c", "e", " ", "d", …}
```

- **并集**

```js
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 2]);
var union = new Set([...a, ...b]); // {1, 2, 3, 4}
```

- **交集**

```js
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 2]);
var intersect = new Set([...a].filter(x => b.has(x))); // {2, 3}
```

- **差集**

```js
var a = new Set([1, 2, 3]);
var b = new Set([4, 3, 2]);
var difference = new Set([...a].filter(x => !b.has(x))); // {1}
```
