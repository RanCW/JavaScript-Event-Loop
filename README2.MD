### JavaScript中的执行机制

 总所周知JavaScript语言是单线程语言，单线程就意味着所有的任务都需要按序执行，只有上一个任务结束后才能继续执行下一个任务，那JavaScript当中它的执行机制又是怎么样的呢？下面我们就将以代码为例，逐一的来理解。
 
#### JavaScript中的调用堆栈和任务队列
 为了更好的理解JavaScript中的调用堆栈和任务队列，请看下图（转引自Philip Roberts的演讲[《Help, I'm stuck in an event-loop》](http://vimeo.com/96425312)）：
 
   ![image](./3.png)
 
 例子1：
  ```javascript
       console.log(1);
       function test() {
         console.log('test');
       }
       test();
       console.log(3);
       //执行结果：1、test、3
   ```