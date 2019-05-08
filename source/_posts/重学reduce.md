---
title: 重学reduce
date: 2019-05-01 01:25:05
tags: js基础
---

## arr.reduce(callback,initialValue)

## reduce()方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

## callback

| 参数         | 是否必选 | 字段含义                         |
| ------------ | -------- | -------------------------------- |
| accumulator  | 必需     | 初始值(initialValue)或数组的首项 |
| currentValue | 必需     | 数组中正在处理的元素             |
| currentIndex | 可选     | 数组中正在处理的当前元素的索引   |
| array        | 可选     | 调用 reduce()的数组              |

## initialValue

### initialValue 作为第一次调用 callback 函数时的第一个参数(accumulator)的值。

### 当 reduce 没有 initialValue 时,accumulator 的值是数组的首项的值,currentValue 是数组第二项的值,currentValue 为 1 ,如果是空数组,reduce 会报错。

### 当 reduce 有 initialValue 时,accumulator 等于 initialValue,currentValue 是数组首项的值,currentValue 为 0 。

## 例子

### 数组求和

```javascript
const total = [0, 1, 2, 3].reduce((acc, cur) => {
  return acc + cur;
});
```

### 数组降维

```javascript
const flattened = [[0, 1, 2, 3], [2, 3, 5, 6], [4, 5]].reduce(
  (acc, cur) => acc.concat(cur),
  []
);
```

### 字符串中的字符出现次数

```javascript
function countChartTime(param) {
  let arr = [];
  if (Object.prototype.toString.call(param) === "[object String]") {
    arr = param.split("");
  } else if (Object.prototype.toString.call(param) === "[object Array]") {
    arr = param;
  } else {
    throw new TypeError(`${fn} is not a String or Array`);
  }
  const countChart = arr.reduce((accumulator, currentValue) => {
    if (currentValue in accumulator) {
      accumulator[currentValue]++;
    } else {
      accumulator[currentValue] = 1;
    }
    return accumulator;
  }, {});
  return countChart;
}
```

### 数组去重

```javascript
let arr = [1, 2, 1, 2, 3, 5, 4, 5, 3, 4, 4, 4, 4];
let result = arr.sort().reduce((init, current) => {
  if (init.length === 0 || init[init.length - 1] !== current) {
    init.push(current);
  }
  return init;
}, []);
```

## 扩展

### 用 reduce() 实现 Map()

```javascript
Array.prototype.imitateMap = function(fn) {
  if (Object.prototype.toString.call(fn) !== "[object Function]") {
    throw new TypeError(`${fn} is not a function`);
  }
  return this.reduce((pre, cur, index, array) => {
    pre.push(fn.bind(this)(cur, index, array));
    return pre;
  }, []);
};
```
