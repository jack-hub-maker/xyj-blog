# 排序算法

## 一、冒泡排序

- 比较所有相邻的元素，如果第一个比第二个大，则它们两个交换位置
- 一轮下来，可以保证最后一个数是最大的
- 执行 n-1 轮，就可以完成排序

代码实现

```js
Array.prototype.bubbleSort = function () {
  // 执行n-1轮
  for (let i = 0; i < this.length - 1; i++) {
    // 一轮循环结束完了,最后一个就不参与下一轮循环了(-i)
    for (let j = 0; j < this.length - 1 - i; j++) {
      if (this[j] > this[j + 1]) {
        const temp = this[j];
        this[j] = this[j + 1];
        this[j + 1] = temp;
      }
    }
  }
  return this;
};

const arr = [5, 4, 3, 2, 1];
const newArr = arr.bubbleSort();
console.log(newArr);
```

时间复杂度

- 两个嵌套循环
- o(n<sup>2</sup>)
- 性能较差

## 二、选择排序

- 找到数组中最小值,选中它放在第一位
- 接着找到第二小的值,选中它放在第二位
- 以此类推,执行 n-1 轮

代码实现

```js
Array.prototype.selectionSort = function () {
  // 需要循环n-1轮
  for (let i = 0; i < this.length - 1; i++) {
    // 第二轮循环后续的操作都需要从第二轮开始了,依次类推
    let indexMin = i;
    for (let j = i; j < this.length; j++) {
      // 拿到最小位数的下标
      if (this[j] < this[indexMin]) {
        indexMin = j;
      }
    }
    // 自己不能跟自己调换位置
    if (indexMin !== i) {
      const temp = this[i];
      this[i] = this[indexMin];
      this[indexMin] = temp;
    }
  }
  return this;
};

const arr = [5, 4, 3, 2, 1];
const newArr = arr.selectionSort();
console.log(newArr);
```

时间复杂度

- 两个嵌套循环
- o(n<sup>2</sup>)
- 性能较差
