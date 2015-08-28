## Grand Central Dispatch

- 任务和队列
    + 任务：执行什么操作
    + 队列：用来存放任务

- 如何执行任务
    + 同步函数dispatch_sync
        * 不具备开启新线程的能力
    + 异步函数dispatch_async
        * 具备开启新线程的能力
    + 同步和异步主要影响：能不能开启新的线程

- 队列的类型
    + 并发队列
        * 可以让多个任务并发（同时）执行
        * 自己创建: dispatch_queue_t queue = dispatch_queue_create("yrion.plus.ios", DISPATCH_QUEUE_CONCURRENT);
        * 全局并发队列 : dispatch_get_global_queue(0 , 0);
    + 串行队列
        * 让任务一个接着一个地执行
        * 自己创建:dispatch_queue_create("yrion.plus.ios", DISPATCH_QUEUE_SERIAL)
        * 主队列:dispatch_get_main_queue()
    + 并发和串行主要影响：任务的执行方式

- GCD的各种组合
    + 异步 +  并行 = 会开启新的线程
        * 异步函数, 会先执行完所有的代码, 再在子线程中执行任务
    + 异步 + 串行 = 会创建新的线程, 但是只会创建一个新的线程, 所有的任务都在这一个新的线程中执行
    + 同步 + 并行 = 不会开启新的线程
        * 其实就相当于同步 + 串行
        * 同步函数, 只要代码执行到了同步函数的那一行, 就会立即执行任务, 只有任务执行完毕才会继续往后执行
    + 同步 + 串行 = 不会创建新的线程
    + 异步 + 主队列 = 不会开启新的线程
        * 只要是主队列, 永远都在主线程中执行
    + 同步 + 主队列 = 需要记住的就一点: 同步函数不能搭配主队列使用
        * 注意: 有例外的情况, 如果同步函数是在异步函数中调用的, 那么没有任何问题
    + 详见备课代码


- GCD线程间通信
    + 利用异步函数执行任务
    + 利用主队列回到主线程更新UI


- GCD中的常用方法
- 延迟执行

```
	// [NSTimer scheduledTimerWithTimeInterval:2.0 target:self selector:@selector(run) userInfo:nil repeats:NO];

    // 内部实现原理就是NSTimer
    //    [self performSelector:@selector(run) withObject:nil afterDelay:2.0];

    // 将需要执行的代码, 和方法放在一起, 提高代码的阅读性
    // 相比NSTimer来说, GCD的延迟执行更加准确
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        NSLog(@"%@", [NSThread currentThread]);
        NSLog(@"run");

```

- 一次性代码 

```
    1) 整个程序运行过程中, 只会执行一次
    2) 用于单例模式的实现
    	* 利用GCD的方法(dispatch_once)实现单例模式 -> 重写allocWithZone方法
    	//	代码
		static dispatch_once_t onceToken;
		    dispatch_once(&onceToken, ^{
		    NSLog(@"我被执行了");
		    });
    	* 用 #if __has_feature(objc_arc) 判断当前是否为ARC
 		* 对于MRC的情况,需要考虑以下的情况
    		1. retain 如果对象经过深拷贝,仍会生成新的对象 -> 应该不会生成新的对象,因为新的对象也需要调用allocWithZone方法
    		2. release 会将唯一的对象释放,导致程序崩溃
    		3. retainCount 的的返回值一般都是MAXFLOAT,或者是负数
    	* 实现单例的类不能继承
    	* 利用宏定义实现单例代码的复用
```

- 快速迭代

```
	/*
     第一个参数: 需要执行几次任务
     第二个参数: 队列
     第三个参数: 当前被执行到得任务的索引
     */
    /*
     dispatch_apply(10, dispatch_get_global_queue(0, 0), ^(size_t index) {
     NSLog(@"%@, %zd",[NSThread currentThread] , index);
     });
     */
```

- barrier
    - 要想执行完前面所有的任务再执行barrier必须满足两个条件
        * 所有任务都是在同一个队列中
        * `队列不能是全局并行队列, 必须是自己创建的队列`
    - barrier方法之前添加的任务会先被执行,只有等barrier方法之前添加的任务执行完毕,才会执行barrier
    - 而且如果是在barrier方法之后添加的任务,必须等barrier方法执行完毕之后才会开始执行

- group
    - 如果想实现,等前面所有的任务都执行完毕,再执行某一个特定的任务,那么可以通过GCD中年的组来实现
    - 只要当前组中所有的任务都执行完毕了,那么系统会自动调用dispatch_group_notify