##帧动画
[UIImageView](uiimageview.md)

##渐变动画
- 属性：
	- alpha `设置控件整体的透明度`
	- opacity `用来设置背景的透明度,通过颜色调整`
	
	```
	UILabel *tipLabel = [[UILabel alloc] initWithFrame: CGRectMake(0, 0, 100, 30)];
    tipLabel.center = CGPointMake(self.view.center.x, self.view.center.y);
    tipLabel.font = [UIFont systemFontOfSize:13];
    tipLabel.textAlignment = NSTextAlignmentCenter;
    tipLabel.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.3];
    tipLabel.alpha = 0.0;
	```
	
##实现方式
```
//UIView 自带方法
//第一种
+ (void)beginAnimations:(NSString *)animationID context:(void *)context;
+ (void)setAnimationDuration:(NSTimeInterval)duration; 
+ (void)commitAnimations;
//第二种
+ (void)animateWithDuration:(NSTimeInterval)duration delay:(NSTimeInterval)delay options:(UIViewAnimationOptions)options animations:(void (^)(void))animations completion:(void (^)(BOOL finished))completion;
```