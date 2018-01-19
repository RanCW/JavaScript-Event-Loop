### JavaScript中的JS引擎的执行机制

  总所周知JavaScript语言是单线程语言，单线程就意味着所有的任务都是排队按序执行的，那到底是不是这样子呢？下面我们先看一段代码：
  ```javascript
      console.log(1);
      function test() {
        console.log('test');
      }
      test();
      console.log(3);
      //执行结果：1、test、3
  ```
  这样子看起来，按序执行的貌似是对的，但是真的是这样子吗？我们再来看下面这样一段代码：
  ```javascript
    console.log(1);
    function test() {
      setTimeout(function () {
        console.log('test');
      })
    }
    test();
    console.log(3);
    //执行结果：1、3、test

```
如果说所有的任务都是一样的排队执行，那上面的代码貌似输出结果也还是一样的——1、test、3，但是这里很不巧的是输出了——1、3、test，那JavaScript里面的执行机制到底是怎么样的呢？下面就让我们一起来揭开JavaScript中执行机制的神秘面纱吧！
  
  
### 什么是异步模式？
   