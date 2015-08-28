##Quartz
###绘图步骤
- 获取上下文

```
Bitmap Graphics Context
PDF Graphics Context
Window Graphics Context
Layer Graphics Context  **只能在draw:中获得,其他情况都是nil,无法自己开启一个**
Printer Graphics Context
```
- 构造路径 - 常用方法

```
//新建一个起点
void CGContextMoveToPoint(CGContextRef c, CGFloat x, CGFloat y)

//添加新的线段到某个点
void CGContextAddLineToPoint(CGContextRef c, CGFloat x, CGFloat y)

//添加一个矩形
void CGContextAddRect(CGContextRef c, CGRect rect)

//添加一个椭圆
void CGContextAddEllipseInRect(CGContextRef context, CGRect rect)

//添加一个圆弧
void CGContextAddArc(CGContextRef c, CGFloat x, CGFloat y,
  CGFloat radius, CGFloat startAngle, CGFloat endAngle, int clockwise)
```
- 将路径添加到上下文(可以上个步骤合并)
- 渲染路径

**总结**

	1. 首先，得有图形上下文，因为它能保存绘图信息，并且决定着绘制到什么地方去
	2. 其次，那个图形上下文必须跟view相关联，才能将内容绘制到view上面
	自定义view的步骤:
	新建一个类，继承自UIView
	实现- (void)drawRect:(CGRect)rect方法,然后在这个方法中
	取得跟当前view相关联的图形上下文
	绘制相应的图形内容
	利用图形上下文将绘制的所有内容渲染显示到view上面
	
	View内部有个layer（图层）属性,drawRect:方法中取得的是一个Layer Graphics Context,因此绘制的东西其实是绘制到view的layer上去了

##矩阵操作
```
利用矩阵操作，能让绘制到上下文中的所有路径一起发生变化
缩放
void CGContextScaleCTM(CGContextRef c, CGFloat sx, CGFloat sy)

旋转
void CGContextRotateCTM(CGContextRef c, CGFloat angle)

平移
void CGContextTranslateCTM(CGContextRef c, CGFloat tx, CGFloat ty)
```

##其他常用操作注意点
- 手势解锁
	- 如果用代码添加到父控件上,需要将backgroundColor设置为clearColor
- 截图
	- 需要考虑图片失帧的问题