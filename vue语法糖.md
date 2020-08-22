# vue语法糖

### 1、v-bind动态绑定（对象、数组）为：“:”。

- ##### 用法举例：

  - ```html
    <img v-bind:src="imgUrl" alt="">
    <img :src="imgUrl" alt="">
    ```

### 2、v-on事件监听为：“@”。

- ##### 用法举例：

  - ```html
    <br>
    <button v-on:click="counter++">+</button>
    <button v-on:click="counter--">-</button>
    <br>
    <button @click="counter++">+</button>
    <button @click="counter--">-</button>
    ```
  
  

