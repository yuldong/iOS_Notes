## NSOperation
- "抽象类" -> 使用其子类或者自定义NSOperation类

#### NSBlockOperation
`第一个创建的block默认在主线程中执行`

`后续追加的则会开启子线程执行`

#### NSInvocationOperation
`在主线程中执行`

**详细的使用可参考对应的类文件**

## NSOperationQueue
- mainQueue

- 自己创建的队列
	- 开启并行执行 -> 子线程数量由系统决定
	- 通过设置maxConcurrentOperationCount = 1控制队列执行方式 -> `仿`串行
- 队列的状态
	- 设置`suspended`来控制后续尚未执行的NSOperation任务状态 `对主队列无效`		- 可恢复
	- 通过发送 cancelAllOperations 消息取消后续NSOperation的执行 `对主队列无效`

`说明:不能控制当前执行的NSOperation任务,取消的任务无法撤销`

`如果自定义操作中做了很多耗时操作,苹果建议定期检查是否已经取消了`


- 队列中任务之间的依赖
    - 在任务添加到队列之前,通过任务发送addDependency消息添加依赖关系
    - 添加依赖之后,只有所有依赖的任务都执行完毕,才会执行当前任务
    
`注意点:不要相互依赖
 特点: 跨队列依赖(GCD默认是不支持)
`

```
 	//	添加依赖
    [op5 addDependency:op1];
    [op5 addDependency:op2];
    [op5 addDependency:op3];
    [op5 addDependency:op4];
```

- 任务的监听
    - 只需要通过任务发送completionBlock消息即可
    - 只要任务执行完毕,就会回调completionBlock

- 线程间的通信
    - 将任务添加到自己创建的队列中
    - 再利用mainQueue回到主队列