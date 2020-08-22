# vue高阶函数

### 1.filter函数的使用

~~~javascript
const nums = [10, 20, 111, 222, 444, 40, 50];
let newNums = nums.filter(function (n) {
   return n < 100;
});
console.log(newNums);

//结果集合newNums：[10, 20, 40, 50]
~~~

### 2、map函数的使用

~~~javascript
const nums = [10, 20, 40, 50];
let new2Nums = newNums.map(function (n) { // 20
  return n * 2;
})
console.log(new2Nums);

//结果集合new2Nums：[20, 40, 80, 100]
~~~

### 3、reduce函数的使用：对数组中所有的内容进行汇总

~~~javascript
const new2Nums = [20, 40, 80, 100];
let total = new2Nums.reduce(function (preValue, n) {
  return preValue + n;
}, 0)
console.log(total);

//计算过程：
//第一次: preValue=0, n=20
//第二次: preValue=20, n=40
//第三次: preValue=60, n=80
//第四次: preValue=140, n=100

//输出结果：240
~~~

### 4、链式语法

- #### 写法1：

~~~javascript
const nums = [10, 20, 111, 222, 444, 40, 50];
let total = nums.filter(function (n) {
  return n < 100;
}).map(function (n) {
  return n * 2;
}).reduce(function (prevValue, n) {
  return prevValue + n;
}, 0);
console.log(total);

//输出结果：240
~~~

- #### 写法2：

~~~javascript
const nums = [10, 20, 111, 222, 444, 40, 50];
let total = nums.filter(n => n < 100).map(n => n * 2).reduce((pre, n) => pre + n);
console.log(total);
~~~



