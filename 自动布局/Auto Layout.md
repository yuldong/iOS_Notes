#Formula
##核心公式
object1.property1 = (object2.property2 * multiplier) + constant value

```
//代码实现
+(id)constraintWithItem:(id)view1 attribute:(NSLayoutAttribute)attr1 relatedBy:(NSLayoutRelation)relation toItem:(id)view2 attribute:(NSLayoutAttribute)attr2 multiplier:(CGFloat)multiplier constant:(CGFloat)c;
```

##警告
- 当前控件的位置与设置的约束存在冲突

##错误
- 缺乏必要的约束
	- 只约束了宽度和高度,没有约束具体的位置
- 约束冲突
	- 对于同一个属性设置了多次约束
	
##代码实现
- 禁止控件的Autoresizing功能
	- `translatesAutoresizingMaskIntoConstraints`
- 不用给view设置frame
- 添加约束之前,要保证相关控件都已经在各自的父控件上
- 代码实现
	- 利用NSLayoutConstraint类创建具体的约束对象
	- 将约束对象添加到相应的对象上
	
	```
	- (void)addConstraint:(NSLayoutConstraint *)constraint;
	- (void)addConstraints:(NSArray *)constraints;
	```
- 约束添加规则

	- if the constraint is between two views that sit on a common immediate parent view,meaning that both these iews have the same superviews, add the constraints to the parent view.
	- if the constraint is between a view and its parent view, add the constraint to the parent view.
	- if the constraints is between two views that do not share the same parent view, add the constraint to the common ancestor of the views.

- 修改约束
	- 更新
		
		如果重写了 `updateViewConstraints` 方法,则需要先调用*`[self.view setNeedsUpdateConstraints]`*,而且必须在重写的`updateViewConstraints`方法中使用[super updateViewConstraints]完成更新;
		
		[self.view updateConstraintsIfNeeded]; //可不写,自动调用,显示调用时会立刻调用重写的`updateViewConstraints`方法
	- 修改
		需要删除已存在的属性约束,再重新设置
	
		[self.view layoutIfNeeded];//可不写,一般实现动画效果会显示调用
		
		```
		[UIView animateWithDuration:0.1 animations:^{
        	[self.view layoutIfNeeded];
    	} ];
		```
##实现在SuperView中等间距
- 在需要等间距的地方增加子控件
- 设置子控件的属性hidden
- 设置子控件与实际显示控件的关系(即设置约束)

