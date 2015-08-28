##AlertView
- 弹窗,普通提示信息
- 通过代理监听用户点击以处理事件

`注意:项目中通过 - (NSString *)buttonTitleAtIndex:(NSInteger)buttonIndex 方法判断点击的按钮,而不应该根据索引去判断点击的按钮`

- Xcode 推荐使用AlertController 

##ActionSheet
- 底部弹窗,重要提示信息,通过设置*UIActionSheetStyle*属性标识
- 通过代理监听用户点击以处理事件

`注意:项目中通过 - (NSString *)buttonTitleAtIndex:(NSInteger)buttonIndex 方法判断点击的按钮,而不应该根据索引去判断点击的按钮`

- Xcode 推荐使用AlertController 

##AlertController
- IOS8.0 新增类,通过设置**UIAlertControllerStyle**来实现上述两种类的效果
- UITextField只能添加到`UIAlertControllerStyleAlert`该种Controller中

```
//
//  UIAlertController.h
//  UIKit
//
//  Copyright (c) 2014 Apple Inc. All rights reserved.
//

#import <UIKit/UIViewController.h>

typedef NS_ENUM(NSInteger, UIAlertActionStyle) {
    UIAlertActionStyleDefault = 0,
    UIAlertActionStyleCancel,
    UIAlertActionStyleDestructive
} NS_ENUM_AVAILABLE_IOS(8_0);

typedef NS_ENUM(NSInteger, UIAlertControllerStyle) {
    UIAlertControllerStyleActionSheet = 0,
    UIAlertControllerStyleAlert
} NS_ENUM_AVAILABLE_IOS(8_0);

NS_CLASS_AVAILABLE_IOS(8_0) @interface UIAlertAction : NSObject <NSCopying>

+ (instancetype)actionWithTitle:(NSString *)title style:(UIAlertActionStyle)style handler:(void (^)(UIAlertAction *action))handler;

@property (nonatomic, readonly) NSString *title;
@property (nonatomic, readonly) UIAlertActionStyle style;
@property (nonatomic, getter=isEnabled) BOOL enabled;

@end

NS_CLASS_AVAILABLE_IOS(8_0) @interface UIAlertController : UIViewController

+ (instancetype)alertControllerWithTitle:(NSString *)title message:(NSString *)message preferredStyle:(UIAlertControllerStyle)preferredStyle;

- (void)addAction:(UIAlertAction *)action;
@property (nonatomic, readonly) NSArray *actions;
- (void)addTextFieldWithConfigurationHandler:(void (^)(UITextField *textField))configurationHandler;
@property (nonatomic, readonly) NSArray *textFields;

@property (nonatomic, copy) NSString *title;
@property (nonatomic, copy) NSString *message;

@property (nonatomic, readonly) UIAlertControllerStyle preferredStyle;

@end

```