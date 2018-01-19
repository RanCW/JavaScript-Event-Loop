### JavaScript中的执行机制

 总所周知JavaScript语言是单线程语言，单线程就意味着所有的任务都需要按序执行，只有上一个任务结束后才能继续执行下一个任务，那JavaScript当中它的执行机制又是怎么样的呢？下面我们就将以代码为例，逐一的来理解。
 
#### JavaScript中的调用堆栈和任务队列
 为了更好的理解JavaScript中的调用堆栈和任务队列，请看下图（转引自Philip Roberts的演讲[《Help, I'm stuck in an event-loop》](http://vimeo.com/96425312)）：
 
   ![image](./3.png)
   
  以上图说明主线程在执行的时候产生堆（heap）和栈（stack），当执行环境的堆栈中的一个任务（task）在执行的时候，其它的任务都要处于等待状态。当主进程执行到异步操作的时候就会将异步操作对应的task回调放置到对应的任务队列中，当主进程的调用堆栈中所有的task都执行完成后再去执行任务队列当中的task（回调函数）；如下：
 
 例子1：
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
   以上代码的执行如下图所示：
   ![image](./1.png)