# 自定义深拷贝函数

## 一、前言

前面我们已经学习了对象相互赋值的一些关系，分别包括：
- 引入的赋值：指向同一个对象，相互之间会影响；
- 对象的浅拷贝：只是浅层的拷贝，内部引入对象时，依然会相互影响；
- 对象的深拷贝：两个对象不再有任何关系，不会相互影响；


并且在JSON专栏我们已经可以通过一种方法来实现深拷贝了：JSON.parse
- 这种深拷贝的方式其实对于函数、Symbol等是无法处理的；
- 并且如果存在对象的循环引用，也会报错的；

## 二、基本实现

```js

```