# UIImageView
## 动画相关属性
 ```objc
 @property(nonatomic,copy) NSArray *animationImages;
 // The array must contain UIImages.
 // Setting hides the single image. default is nil

@property(nonatomic) NSTimeInterval animationDuration;
// for one cycle of images.
// default is number of images * 1/30th of a second
// (i.e. 30 fps)

@property(nonatomic) NSInteger      animationRepeatCount;
// 0 means infinite (default is 0)
 ```
 ## 动画相关方法
```objc
- (void)startAnimating;

- (void)stopAnimating;

- (BOOL)isAnimating;
```
## 注意事项
- 缓存问题

```objc
// imageNamed: 有缓存(传入文件名)
// UIImage *image = [UIImage imageNamed:filename];

// imageWithContentsOfFile: 没有缓存(传入文件的全路径)
NSBundle *bundle = [NSBundle mainBundle];
NSString *path = [bundle pathForResource:filename ofType:nil];
UIImage *image = [UIImage imageWithContentsOfFile:path];
```
- 动画执行的时间以及次数

```objc
    //动画播放次数
    self.tom.animationRepeatCount = 10;
    // 设置播放时间,该时间为播放一次动画的时间
    self.tom.animationDuration = images.count * 0.05;
    [self.tom startAnimating];
    // 动画放完1秒后清除内存(或者执行其他方法),此处设置的时间应该是动画所有次数播放完成的时间
    CGFloat delay = self.tom.animationRepeatCount * self.tom.animationDuration + 1.0;
    [self.tom performSelector:@selector(setAnimationImages:) withObject:nil afterDelay:delay];
```
