##事件触发原因:继承自UIResponder
**`分类`**
#####触摸事件
- 自定义控件(UIView)并重写如下方法

```
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;
```

- 参数 NSSet -> touches
	- touches是UITouch对象集合,通过两个核心方法获取点坐标
	
	`注意,坐标点都是相对当前自定义控件的坐标系`
	
	```
	// 返回值表示触摸在view上的位置
	// 这里返回的位置是针对view的坐标系的（以view的左上角为原点(0, 0)）
	// 调用时传入的view参数为nil的话，返回的是触摸点在UIWindow的位置
	- (CGPoint)locationInView:(UIView *)view;

	//获取手指上个点的坐标
	- (CGPoint)previousLocationInView:(UIView *)view;
	```
- 参数 UIEvent -> event
	- 事件对象,记录事件产生的时刻和类型
	
#####加速计事件
#####远程操控事件

##事件的产生与传递
- 发生触摸事件后,系统会将事件加入到一个由`UIApplication`管理的**事件队列**中
- UIApplication会从事件队列中取出最前面的事件,并将事件分发下去,通常,先发送事件给应用程序的KeyWindow
- 主窗口会在视图层次结构中找到一个最合适的视图来处理触摸事件,这也是整个事件处理的第一步
- 找到合适的视图后,执行相应的touch事件

	**查找合适视图的逻辑**
	
	```
	- 确认自己是否能接收触摸事件
	- 确认触摸点是否在自己身上
	- 从后往前遍历子控件，重复前面的两个步骤
	- 如果没有符合条件的子控件，那么就自己最适合处理
	```
`注意点:如果父控件无法接受事件,那么子控件就不可能接受事件,UIView不接受事件处理的三种情况`

1. userInteractionEnabled = NO 
2. hidden = YES 
3. alpha = 0 ~ 0.01

###事件产生的底层实现
- 通过如下两个方法判断触摸位置,从而找到合适的视图

```
	- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event;   // recursively calls -pointInside:withEvent:. point is in the receiver's coordinate system
	- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event;   // default returns YES if point is in bounds
```
**第一个方法可用来验证查找合适视图的顺序,返回合适的视图,通过修改返回值可以调整处理时间的对象**

**第二方法用来判断点的位置是否在视图中,第一个方法实际通过该方法判断点的合适归属视图**

`注意点:在第二个方法中,需要将point转成其子控件相应坐标系的点去判断点的归属`

###事件传递-事件处理的详细过程
- 传递过程

```
1> 先将事件对象由上往下传递(由父控件传递给子控件),找到最合适的控件来处理这个事件
2> 调用最合适控件的touch...方法
3> 如果调用了[super touch...];就会将事件顺着响应者链条往上传递,传递给上一个响应者
4> 接着就会调用上一个响应者的touch...方法
```
- 确认响应者

```
1> 如果当前这个view是控制器的view,那么控制器就是上一个响应者
2> 如果当前这个view不是控制器的view,那么父控件就是上一个响应者
```
*事件传递总结*

```
1> 如果view的控制器存在，就传递给控制器;如果控制器不存在,则将其传递给它的父视图
2> 在视图层次结构的最顶级视图,如果也不能处理收到的事件或消息,则其将事件或消息传递给window对象进行处理
3> 如果window对象也不处理,则其将事件或消息传递给UIApplication对象
4> 如果UIApplication也不能处理该事件或消息,则将其丢弃
```
## 手势识别
**注意点:**
 
```
1> A gesture recognizer operates on touches hit-tested to a specific view and all of that view’s subviews. It thus must be associated with that view. To make that association you must call the UIView method addGestureRecognizer:. A gesture recognizer doesn’t participate in the view’s responder chain.
2> Clients that merely use concrete subclasses of UIGestureRecognizer must never call these methods (except for those noted).
```

### 种类
UITapGestureRecognizer `轻巧`

UIPinchGestureRecognizer `捏合`

UIRotationGestureRecognizer `旋转`

UISwipeGestureRecognizer `轻扫`

UIPanGestureRecognizer `拖拽`

UIScreenEdgePanGestureRecognizer 
`A UIScreenEdgePanGestureRecognizer looks for panning (dragging) gestures that start near an edge of the screen. The system uses screen edge gestures in some cases to initiate view controller transitions.`

UILongPressGestureRecognizer `长按`

### 处理手势
- 获得触摸点	`点坐标都是相对当前视图的坐标系 `

```
- (CGPoint)locationInView:(UIView*)view;
```
- 对于不同手势,其子类都提供不同的方法获取视图应该调整的值

- 这种调整都是在上次手势保存的值基础上做的调整`即视图原始值(最原始的状态)做的调整`,需要在每次调整后把手势保存的值清空
- 对同一个View可添加多个swipe,实现不同方向的轻扫
- 对同一个View如果允许不同手势同时作用,则需要实现其中的一个代理方法

```
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer;
```
