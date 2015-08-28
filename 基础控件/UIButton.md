##实现UIButton子控件的布局
### 自定义类 -> 继承自UIButton
- 重写UIButton中的相关方法
	- 第一种方式,重写UIButton中的下面两个方法,此种方式在title与image的frame存在联系时,实现上比较麻烦
	
	```
	- (CGRect)titleRectForContentRect:(CGRect)contentRect;
	- (CGRect)imageRectForContentRect:(CGRect)contentRect;
	```
	
	- 第二种方式,重写UIView中的layoutSubViews方法,该方法是在控件的frame时调用
	
- 注意点
	- 对于分状态的属性，不能直接赋值,需要使用相应的方法
	- 对于可一次性设置的属性,一般在初始化中设置
	- UIEdgeInsets **`需要研究`**
	
### UIButton拉伸的问题
- 调整图片的拉伸模式(UIImage自带的方法)
	- 第一种方式
	
	```
	- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets NS_AVAILABLE_IOS(5_0); // create a resizable version of this image. the interior is tiled when drawn.
	- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets resizingMode:(UIImageResizingMode)resizingMode NS_AVAILABLE_IOS(6_0); // the interior is resized according to the resizingMode		
```
	
	- 第二种方式
	
	```
	- (UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight;
	@property(nonatomic,readonly) NSInteger leftCapWidth;   // default is 0. if non-zero, horiz. stretchable. right cap is calculated as width - leftCapWidth - 1
	@property(nonatomic,readonly) NSInteger topCapHeight;   // default is 0. if non-zero, vert. stretchable. bottom cap is calculated as height - topCapWidth - 1
	```

	