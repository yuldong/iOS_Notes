## NSThread

#### 创建方式
- -(instancetype)initWithTarget:(id)target
                       selector:(SEL)selector
                         object:(id)argument
   
   `优点:可以对分配的thread进行详细的属性设置,比如 name,threadPriority等`
   
   `缺点:需要显示发送start消息`
   
   **同一个线程不能发送两次start消息**
   
   **一个线程默认执行一次,对于执行后的线程会自动释放 -> 可自定义NSThread并实现dealloc方法进行测试**
   
   **对自定义NSThread,发送start消息会自动调用其中的main(),所以一般都会重写main方法**
- +(void)detachNewThreadSelector:(SEL)aSelector
                        toTarget:(id)aTarget
                      withObject:(id)anArgument
 
 	`优点:直接开启新线程执行aSelector`
   
    `缺点:无法对属性进行设置`
    
#### 常用方法

```
+ (NSThread *)mainThread
+ (NSThread *)currentThread

+ (void)sleepUntilDate:(NSDate *)aDate
+ (void)sleepForTimeInterval:(NSTimeInterval)ti

- (void)start
- (void)cancel

+ (void)exit
```
`说明:thread再被强制停止后(发送exit消息),不会再执行后续的代码`

#### 与其他线程的通讯方式

```
- (void)performSelectorOnMainThread:(SEL)aSelector withObject:(id)arg waitUntilDone:(BOOL)wait modes:(NSArray *)array;

// equivalent to the first method with kCFRunLoopCommonModes
- (void)performSelectorOnMainThread:(SEL)aSelector withObject:(id)arg waitUntilDone:(BOOL)wait;

- (void)performSelector:(SEL)aSelector onThread:(NSThread *)thr withObject:(id)arg waitUntilDone:(BOOL)wait modes:(NSArray *)array;

// equivalent to the first method with kCFRunLoopCommonModes
- (void)performSelector:(SEL)aSelector onThread:(NSThread *)thr withObject:(id)arg waitUntilDone:(BOOL)wait NS_AVAILABLE(10_5, 2_0);

- (void)performSelectorInBackground:(SEL)aSelector withObject:(id)arg NS_AVAILABLE(10_5, 2_0);
```
`说明:**waitUntilDone**该参数决定aSelector的执行方式 -> 即是否等待Thread的所有操作完成再执行aSelector的代码`

`如果想实现子线程之间的通信,则必须主动创建子线程的RunLoop并保证其不会被关闭`

#### 互斥锁
- 应用场景:多线程存在资源抢夺(多个线程同时操作(写操作)某个文件/变量等)
- 注意点:
    - 只要枷锁就会消耗性能
    - `多个线程必须使用同一把锁(锁定同一个对象)才行`
    - 加锁的时候尽量缩小范围, 因为范围越大性能就越低

	```
	@synchronized(self) {
	// 需要被锁定的代码
	}
	```

- 原子和非原子属性
    - `atomic:原子属性,为setter方法加锁`（默认就是atomic）
    - nonatomic:非原子属性,不会为setter方法加锁
     
`说明:atomic是系统自动给我们添加的锁,不是互斥锁而是自旋锁`

- 自旋锁和互斥锁对比
    - 共同点
        - 都能够保证多线程在同一时候,只能有一个线程操作锁定的代码
    - 不同点
        - 如果是互斥锁,假如现在被锁住了,那么后面来得线程就会进入”休眠”状态,直到解锁之后,又会唤醒线程继续执行
        - 如果是自旋锁,假如现在被锁住了,那么后面来得线程不会进入休眠状态,会一直傻傻的等待,直到解锁之后立刻执行
        - 自旋锁更适合做一些较短的操作

`说明:由于执行的方式不同,所以对于卖票的例子,互斥锁可以保证票数按照顺序打印,而自旋锁则不行`