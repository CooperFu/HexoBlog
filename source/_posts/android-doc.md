title: 无二之旅-Android 技术栈 
date: 2016-01-22 16:25:41
tags:
---
# Android 技术栈

## 架构

旅行君/无二之旅 均采用MVC架构

此模式是将应用层中的业务逻辑抽象出来,这在Android中十分重要.因为Android本身的框架将这一层与数据层耦合在了一起,一个明显的例子就是Adapter.也是官方推荐的方式.

## 开发环境
- 开发工具 [Android Studio](http://tools.android.com/recent) 
- 版本控制 [Git](https://git-scm.com/) 
- Self-hosting Github [GitLab](http://git.uniqueway.in/) 
- 项目依赖管理 [Gradle](http://gradle.org/)


## Library

- Google官方support包[Support Design Library](http://developer.android.com/intl/zh-cn/tools/support-library/index.html)
- 方法超过65535[multidex](http://developer.android.com/intl/zh-cn/tools/building/multidex.html)
- 网络请求库[retrofit](https://github.com/square/retrofit)
- rxjava[RxJava](https://github.com/ReactiveX/RxJava)
- 事件总线[eventbus](https://github.com/greenrobot/EventBus)
- 图片加载[fresco](https://github.com/facebook/fresco)
- Log输出[logger](https://github.com/orhanobut/logger)
- 地图[Mapbox](https://www.mapbox.com/android-sdk/)
- 支付[Ping++](https://www.pingxx.com/)
- 测试框架[robotium](https://github.com/robotiumtech/robotium)


## 协作工具

- Teambition[Teambition](https://www.teambition.com/projects)
- BearyChat[BearyChat](https://bearychat.kf5.com/)