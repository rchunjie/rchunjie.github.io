### 关于子项目开发

#### 一 子项目作为一个独立开发的工程存在，可以单独的思想你需要的功能！

##### 注意事项：

1> 不能和主工程或者任何的子工程有重复的文件名

2> 主工程引用子工程的文件时用#import <>引用

3> 主工程如果想引用子工程进入工程需要在Podfile 中引用  例如：CommonItemModule（网络工程命名） 'git@114.215.238.72:modulerefactor/commonitemmodule.git'（网络地址）

```
pod 'CommonItemModule', :git => 'git@114.215.238.72:modulerefactor/commonitemmodule.git'
```

## 二 如果主工程中想要引入子工程中需要的文件那么就要在子工程podspec文件中设置，设置如下


```

#
#  Be sure to run `pod spec lint VideoDemo.podspec' to ensure this is a
#  valid spec and to remove all comments including this before submitting the spec.
#
#  To learn more about Podspec attributes see http://docs.cocoapods.org/specification.html
#  To see working Podspecs in the CocoaPods repo see https://github.com/CocoaPods/Specs/
#

Pod::Spec.new do |s|

s.name         = "ImContact" //工程名字
s.version      = "1.0.0" //工程版本
s.summary      = "A short description of ImContact." //工程总结
s.source_files  = "ImContact/Class/*.{h,m}" //源文件.h .m
s.resources = "IMContact/Resources/*" //资源文件
s.requires_arc = true         
s.ios.deployment_target = "7.0" //最低系统要求
s.homepage         = ‘https://github.com/<GITHUB_USERNAME>/ImContact' //主页在1.0.0以后必须写入
s.author           = "ShanWen" //作者在1.0.0以后必须写入
s.source           = { :git => 'git@114.215.238.72:modulerefactor/imcontact.git', :tag =>s.version.to_s } //源 1.0.0以后必须写入
s.dependency 'tinyxml', '~> 2.1.0'  //需要的依赖第三方依赖，也就是说主工程不需要加入这句pod就可以下载了
s.libraries = 'z','stdc++.6','sqlite3.0','c++' //依赖系统库libz.tbd,libstdc.6.tbd,libsqlite3.0.tbd,libc++.tbd    

end

```
### 注：//后为解释

## 三 工程结构如下图

![](assets/sonOfEngineering.png)


___
 Resources，Class文件夹和上面的podspec文件设置要统一IMContact/Resources/* 资源文件下所有的文件s.source_files  = "ImContact/Class/* .{h,m}"   源文件ImContact/Class/下的.h .m
 ___
