##UITableView
###使用UIViewController做代理
- 设置UITableViewDataSource,UITableViewDelegate
	- UITableViewDataSource中必须实现的方法
	
	```
	- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
	
	- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
	```
	`Notice:`
	- should always try to reuse cells by setting each 	cell's reuseIdentifier and querying for available reusable cells with 	dequeueReusableCellWithIdentifier:
	
	- 对于Cell中的属性中,涉及到既可以用UIView又可以用enum设置的,UIView优先级高
	
- UITableViewCell 的构建
	- IOS6.0之前的写法:
	
	```
//1.设置Cell标识
static NSString *CellIdentifier = @"CellIdentifier";
//2.从缓存池中根据Cell标识取
UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];
//3.如果缓存池中没有则需要重新创建(一般仅创建 屏幕显示数量 + 1 次)
if (cell == nil) {
    cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:CellIdentifier];
}
	```
	- IOS6.0之后,实现以下方法,仅需要从缓存池中取即可,不用手动和创建,可通过注册的方式自动创建
	
	```
	- (void)registerNib:(UINib *)nib forCellReuseIdentifier:(NSString *)identifier NS_AVAILABLE_IOS(5_0);
	- (void)registerClass:(Class)cellClass forCellReuseIdentifier:(NSString *)identifier NS_AVAILABLE_IOS(6_0);
	```
	
###使用UITableViewController
- 无需设置代理
- UITableViewCell的构建同上
- 无法代码设置UITableView的Style
	- grouped
	- plain
	
###常用属性

```
// 设置每一行cell的高度
self.tableView.rowHeight = 100;

// 设置每一组头部的高度
self.tableView.sectionHeaderHeight = 50;
// 设置每一组尾部的高度
self.tableView.sectionFooterHeight = 50;

// 设置分割线颜色
self.tableView.separatorColor = [UIColor redColor];
// 设置分割线样式
self.tableView.separatorStyle = UITableViewCellSeparatorStyleNone;

// 设置的表头以及表尾的高度在第一次设置tableView后就不会再更改了
// 后续表头或者表尾的尺寸调整不会立刻反映到已设置好的tableview,导致tableview拖动出现问题
// 设置表头控件
self.tableView.tableHeaderView = [[UISwitch alloc] init];
// 设置表尾控件
self.tableView.tableFooterView = [UIButton buttonWithType:UIButtonTypeContactAdd];
//ios 8之后实现不等高Cell的属性设置
self.tableView.estimatedRowHeight = 100;
self.tableView.rowHeight = UITableViewAutomaticDimension;
//设置tableview的编辑状态
self.tableView.editing = YES;
self.tableView.allowsMultipleSelectionDuringEditing = YES;
```

###UITableView 操作 - 删除,新增,修改
- 实现UITableViewDataSource中的方法 **`必须实现`**

```
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath;
```
- 每次修改Cell数据,都需要相应调整模型数据并刷新TableView
- 刷新表格要考虑效率,有以下方法供调用

```
- (void)insertRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
- (void)deleteRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
- (void)reloadRowsAtIndexPaths:(NSArray *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
```