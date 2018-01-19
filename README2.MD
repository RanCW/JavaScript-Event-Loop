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
   
   首先是在执行环境栈中压入执行上下文的main函数，再次是按照顺序执行将console.log(1);压入到执行环境栈中，于是执行环境栈中就有了一个task——console.log(1)，于是并开始执行该task，就输出了1，输出后代码开始继续往执行，得到下图所示环境：

   ![image](./2.png)
   
   在执行环境栈中会加入一个什么test函数的task，于是会声明一个test函数，代码继续往下执行，图示如下：
   
   ![image](./4.png)
   在执行环境栈中会加入一个test()的task,于是会开始执行test(),在执行的时候执行机制如图：
   
   ![image](./5.png)
   test()在执行的时候会执行setTimeout,在执行setTimeout的时候就会创建一个任务队列的task，创建完该task后执行环境栈继续执行，如下图：
   
   ![image](./6.png)
   
  在执行环境栈中会创建一个console.log(3)的task，并执行它，任务队列当中setTimeout创建的task处于等待状态，于是控制台会输出3，那么此时控制台的输出结果当中已经有了**1、3**两个数字，此时执行环境栈中的task已经都执行完成了，执行环境栈出现控制，那么这个时候就会去看任务队列里面的task是否有需要执行的，这个时候setTimeout创建的task就会被发现，该task的执行函数将会被添加到回调队列里面，因为执行环境栈中没有task，于是改回调函数将会被拿到执行环境栈中去执行，如下图所示：
  
  ![image](./7.png)
  
  这时候执行环境栈中的task会开始执行，于是会输出‘test’，输出完成后，执行环境栈、任务队列、回调队列都不存在task，于是整个过程执行完成。
  
#### 
   
   