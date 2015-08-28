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
