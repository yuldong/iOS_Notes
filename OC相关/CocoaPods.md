### CocoaPods

- 安装CocoaPods的步骤:
    + 1.升级一下gem `sudo gem update --system`
    + 2.切换CocoaPods的数据源
    
    ```
    gem sources --remove https://rubygems.org/
    gem sources -a http://ruby.taobao.org/
    gem sources -l
    ```
    + 安装CocoaPods `sudo gem install cocoapods`
    + 将podspec文件托管地址从github替换为国内oschina
    
``` 
   
pod setup在执行时，会输出Setting up CocoaPods master repo，但是会等待比较久的时间。这步其实是Cocoapods在将它的信息下载到 ~/.cocoapods目录下，如果你等太久，可以试着cd到那个目录，用du -sh *来查看下载进度。你也可以参考本文接下来的使用cocoapods的镜像索引一节的内容来提高下载速度。

使用CocoaPods的镜像索引

所有的项目的Podspec文件都托管在https://github.com/CocoaPods/Specs。第一次执行pod setup时，CocoaPods会将这些podspec索引文件更新到本地的 ~/.cocoapods/目录下，这个索引文件比较大，有80M左右。所以第一次更新时非常慢，笔者就更新了将近1个小时才完成。

一个叫akinliu的朋友在gitcafe和oschina上建立了CocoaPods索引库的镜像，因为gitcafe和oschina都是国内的服务器，所以在执行索引更新操作时，会快很多。如下操作可以将CocoaPods设置成使用gitcafe镜像：

```

    ```
    pod repo remove master
    pod repo add master http://git.oschina.net/akuandev/Specs.git
    pod repo update
    ```
    + 设置 pod 仓库 `pod setup`
    + 测试 `pod —version`
    + 如何搜索框架 `pod search name`
    + 如何安装插件 https://github.com/kattrali/cocoapods-xcode-plugin/archive/master.zip
    + 如何查看一个框架是否支持cocoapods
        * 看框架中是否有一个叫做 xxx.podspec文件
    ```
重启电脑按住option别松手等出现一个启动盘的画面按command + r 进入到恢复模式 然后在工具栏找到终端 在终端中输入csrutil disable

