##UITableViewCell
### 自定义Cell - `新建继承自UITableViewCell的类`

#### 等高的Cell - 代码实现
- 重写下面的方法,并在该方法中增加子控件

```
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
```
- 计算子控件的frame
	- 方式一:重写layoutSubviews方法[`必须优先调用[super layoutSubviews]`]
	
	**该方法对于cell高度固定且子控件位置,尺寸固定的情况有瑕疵(仍然会重复的调用)**
	- 方式二:在initWithStyle:reuseIdentifier:中利用AutoLayout方案在第一步的方法中设置约束
	
	`说明:对于Cell高度固定且子控件位置,尺寸固定的情况,推荐第二种方式`

	
#### 等高的Cell - Xib实现
- 在Xib中设置约束(或者frame)
- 更改Xib的Class并设置Identifier


调整控制器中的注册方法：

- 代码实现

```
[self.tableView registerClass:[CustomCell class] forCellReuseIdentifier:@"CellIdentifier"];
```
- Xib实现

```
[self.tableView registerNib:[UINib nibWithNibName:NSStringFromClass([CustomCell class]) bundle:nil] forCellReuseIdentifier:@"CellIdentifier"];
```
####非等高的Cell - 代码实现

- 与实现等高的Cell相同
- 计算子控件的frame,
	- 计算好每个子控件的frame,并在模型中存储,重写layoutSubviews方法[`必须优先调用[super layoutSubviews]`],并在该方法中设置子控件的frame,实现tableview的代理方法设置Cell的高度
	
	```
		- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:		(NSIndexPath *)indexPath{
    		YYStatus *status = self.statuses[indexPath.row];
    		return status.height;
		}
	```
	- 在初始化的方法中设置约束,并在给Cell设置模型数据时调整子控件约束

####非等高的Cell - Xib实现
#####微博为例
- ios 8之后
	- 在Xib或者Storyboard中设置子控件的约束
	- 设置tableview的属性
	
	```
	self.tableView.estimatedRowHeight = 100;
    self.tableView.rowHeight = UITableViewAutomaticDimension;
	```
	**注意点:**
	
	**设置估算高度可以有效地减少系统自动调用计算高度方法的次数**
	
	**对于根据情况进行显示的控件,则需要通过调整其约束`比如微博中的图片,则需要调整其高度约束(调整其frame没有任何效果)`来达到系统自己计算Cell的高度**
	
- ios 8之前或者更早
- 设置子控件的约束
	- 其中对于`显示微博内容的子控件`不能设置其左右约束.
	**注意:系统在计算其高度时,需要设置其属性*self.textInfoLabel.preferredMaxLayoutWidth***
	- 其中的`图片子控件`不能设置底部约束
- 在*- (CGFloat)tableView:heightForRowAtIndexPath:*中手动计算Cell的高度
	- 在计算其高度时的方法如下
	
	```
    [self layoutIfNeeded]; 
    if (self.status.picture) {
        _height = CGRectGetMaxY(self.pictureView.frame) + 5;
    } else {
        _height = CGRectGetMaxY(self.textInfoLabel.frame) + 5;
    }
    return _height;
	```
 - **默认情况下，当一个cell被选中时，cell内部的所有子控件（比如label、imageView）都自动进入highlighted状态**
 - **如果cell的selectionStyle为None，那么cell被选中时，内部的子控件就不会自动进入highlighted状态**