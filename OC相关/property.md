##@property属性的用法
```
- weak(assign): 代理\UI控件\两个对象相互引用时,一端用weak
- strong(retain): 其他对象(除代理\UI控件\字符串以外的对象)
- copy: 字符串,block
- assign: 非对象类型(基本数据类型\枚举\结构体)
```

`使用weak时,如果在给weak对象赋值时将该对象指向强引用,则该对象不会被释放(即使存在警告)`

*Assigning retained object to weak property; object will be released after assignment*

###copy
- 深复制
	- 产生新对象
- 浅复制
	- 等价于strong(引用计数器加 1)

*`multablecopy`* 返回可变字符串 

*`copy`* 返回不可变字符串

**`只有源对象与副本对象都是不可变类型时,才是浅复制,其他情况都是深复制`**

## 控件
 - @property  **`strong,weak`**
    - **官方推荐使用`weak`,因为在UIView中默认有保存这些控件对象的数组,同时控件从父控件移除后，应该从内存中释放**
 - delegate **`weak`**
    - **delegate的代理基本上都是控制器本身,如果使用strong则会造成循环引用的问题**
