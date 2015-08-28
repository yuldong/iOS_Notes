##UIScrollView
###常用属性

```
@property(nonatomic)         CGPoint                      contentOffset;
@property(nonatomic)         CGSize                       contentSize;
@property(nonatomic)         BOOL                         bounces; 
@property(nonatomic,getter=isPagingEnabled) BOOL          pagingEnabled; 
@property(nonatomic,getter=isScrollEnabled) BOOL          scrollEnabled; 
@property(nonatomic,assign) id<UIScrollViewDelegate>      delegate;
```
	
###常用方法
- 动画滚动效果

```
- (void)setContentOffset:(CGPoint)contentOffset animated:(BOOL)animated;
```
- 监听滚动(缩放)事件
	- 设置代理,实现protocol方法

```
//滚动
- (void)scrollViewDidScroll:(UIScrollView *)scrollView;
......
//缩放
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView;
......
```
`注意点:缩放功能,如果有手指不在scrollView范围内,会失去效果`

### 增加循环效果
```
self.timer = [NSTimer scheduledTimerWithTimeInterval:2.0 target:self selector:@selector(nextPage:) userInfo:nil repeats:YES];
[[NSRunLoop mainRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
```
`注意点:避免线程阻塞`

**计时器停止后,即使是强引用,在没有设置`self.timer`为`nil`的情况下,计时器也无法再使用.**

**在停止计时器时,如果self.timer是强引用,则需要将执行 *`self.timer = nil`* 以避免内存泄露**

##UIPageControl
- 继承自UIControl
	- 常用属性
	
		```
		@property(nonatomic) NSInteger numberOfPages;
		@property(nonatomic) NSInteger currentPage; //0..numberOfPages-1
		@property(nonatomic) BOOL hidesForSinglePage;
		```
		`注意点:可通过KVC修改私有变量的值,比如*_currentPageImage,_pageImage* 以调整指示器图片`
	
	**代码创建的pageControl默认size为{0, 0},如果代码没有设置size的值,导致无法触发事件,即使在界面看到pageNumbers,也不过是一种假象**
