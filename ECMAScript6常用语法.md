# ECMAScript6常用语法

### 1、const的使用（常量）

- ##### 什么时候使用const呢？

  当我们修饰的标识符不会被再次赋值时，就可以使用const来保证数据的安全性（不可被修改，即二次赋值）。

- #####  const的注意

  - 注意一：

    ```javascript
    const a = 20;
    a = 30; //错误：不可以修改
    ```
  
  - 注意二：
  
    ```javascript
    const name; //错误：const修饰的标识符必须赋值
    ```
  
  - 注意三：常量的含义是指向的对象不能修改, 但是可以改变对象内部的属性
  
    ```javascript
    const obj = {
        name: 'why',
        age: 18,
        height: 1.88
    };
    
    console.log(obj);
    
    // obj={};//这么写是错误的，会报错
    
    obj.name = 'zs';
    obj.age = 18;
    obj.height = 1.78;
    
    console.log(obj);
    ```
  
- ##### 建议：优先使用const，只有在需要改变某一个标识符的时候才使用let
### 2、let/var

- ##### 事实上var的设计可以看成JavaScript语言设计上的错误. 但是这种错误多半不能修复和移除, 因为需要向后兼容

  - 大概十年前, Brendan Eich就决定修复这个问题, 于是他添加了一个新的关键字：let。

  - 我们可以将let看成更完美的var。

  - 演示代码

    - 变量作用域：变量在什么范围内是可用的

      ```javascript
      // 这段演示代码说明了{}是没有作用域限制的，name在{}块里面能被打印，在{}块外也能被打印
      {
        var name = 'why';
        console.log(name);
      }
      console.log("---", name);
      ```

    - 没有块级作用域引起的问题：if的块级
    
      ```javascript
      var func;
      if (true) {
          var name = 'why';
      
          func = function () {
              console.log(name);
          }
      
          func();
      }
      
      // var是没有块级作用域的（即全局可用）
      // 因此，没有块级作用域限制，这个name的值在任何地方都有可能被随意串改，而且不容易找到是在哪里被串改的
      name = 'ketty';
      
      func();
      ```
    
    - 没有块级作用域引起的问题：for的块级
    
      ```html
      <button>按钮1</button>
      <button>按钮2</button>
      <button>按钮3</button>
      <button>按钮4</button>
      <button>按钮5</button>
      ```
    
      ```javascript
      var btns = document.getElementsByTagName('button');
      
      for (var i = 0; i < btns.length; i++) {
          // btns[i].addEventListener('click', function () {
          //     console.log("第" + i + "个按钮被点击了！");
          // });
          // 上面的for循环运行的结果是：无论点那个按钮，打印结果都是“第5个按钮被点击了”
          // 解决这个问题的方法是在for循环（块）内部使用“闭包”，代码如下
          (function (i) {
              btns[i].addEventListener('click', function () {
                  console.log("第" + i + "个按钮被点击了！");
              });
          })(i);
      }
      // 为什么“闭包”可以解决这个没有作用域的问题呢？
      // 因为函数是一个作用域，一旦入参传入后，就按当前传入的参数计算结果，后续对入参的值的修改，不会改变本次调用传入的入参
      ```
    
    - 总结：JavaScript语法中只有函数是有作用域的，其他的{}、if(){}、for(){}都是没有块级作用域的。在ES6中，加入了let变量， let它是有if和for的块级作用域的。上述“没有块级作用域引起的问题：for的块级”使用let变量的代码演示如下
    
      ```html
      <button>按钮1</button>
      <button>按钮2</button>
      <button>按钮3</button>
      <button>按钮4</button>
      <button>按钮5</button>
      ```
    
      ```javascript
      const btns = document.getElementsByTagName('button');
      
      for (let i = 0; i < btns.length; i++) {
          btns[i].addEventListener('click', function () {
              console.log("第" + i + "个按钮被点击了！");
          });
      }
      ```
    
      
    
      

### 3、对象增强写法

- ##### ES6中，对对象字面量进行了很多增强。

  - 属性的简写

    ```javascript
    // ES6之前
    let name = 'why'
    let age = 18
    let obj1 = {
    	name: name,
    	age: age
    }
    console.log(obj1)
    
    //ES6之后
    let obj2 = {
    	name, age
  }
    console.log(obj2)
    ```
  
  - 方法的简写
  
    ```javascript
    // ES6之前
    let obj1 = {
    	test: function(){
    		console.log('obj1的test函数');
    	}
    }
    obj1.test()
    
    // ES6之后
    let obj2 = {
    	test(){
    		console.log('obj2的test函数');
    	}
    }
    obj2.test()
    ```
  
    
  
  

