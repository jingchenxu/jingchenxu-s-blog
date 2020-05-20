---
description: 最近用flutter把我的小程序重构了一遍，这里主要将flutter的开发体验与微信小程序做一个比较，总结一下是否好用。
---

# flutter 开发总结

## 为什么选择flutter？

作为一个前端开发工程师，怎么能少得了APP端，那就开始吧！一开始我认为react-native可能会是个不错的选择，于是兴冲冲的本地搭建环境，但是最后没成，为什么呢？其实react-native画页面是没问题的，但是生态不是特别的优化，建议技术栈是APP（主）前端（辅）的开发人员可以尝试下，作为前端人员，在没有APP大神的协助，在国内网络的环境下，开发体验不是特别的友好，刚出来的时候挺火的，现在感觉不温不火，也有可能是我没以前那么关注了。

第二个APP端的尝试是uniapp，我也试过，画了几个页面，感觉怎么说了，不爽利，但是作为国内的技术型公司，开发出Hbuilder这样的IDE，我认为这家公司还是不错的，Hbuilder很漂亮，感觉很友好，但是我还是拒绝了，因为我看到了flutter。

其实期间我我尝试过Android原生开发，但是学习周期比较长，GUI的开发逻辑和网页端的开发逻辑还是有比较大的区别的，考虑到自己打算跨平台，所以还是选择flutter吧！

## 关于flutter的传闻

其实决定使用flutter之前，也听到了一些负面消息，主要有下面几点：

* 你要学习一门新的语言Dart，还是与javascript竞争的失败者；
* 用flutter画页面你会陷入可怕的嵌套；
* github上有很多的issues没有关闭。

好吧！我来反驳一下上面的观点，第一点如果你有java开发经验，你一点就不是问题，不知道别人有没有，反正我的主技术栈是java+javascript，所有爽的飞起；说第二点的人多半没有深入的了解flutter，你非要在画页面的时候深层嵌套我也没办法，何况Android studio 和 vscode 都提供不错的代码提示，可以清楚的看清楚层级，如果嵌套过深，你应该考虑将代码进行拆分，将部分页面拆分为组件，而不是堆在一起；至于最后一点，我觉得关闭的速度已经够快了。

## 会遇到的问题

安装的教程我也不打算写，只要电脑是翻墙的，从零到项目模拟器运行应该是行云流水的，网不好的可能会慢点，单基本没什么比较大的坑，下面我主要说下在开发过程中遇到的问题：

### 快速命令行打开ios模拟器

```text
open -a Simulator
```

### 在不同电脑上运行项目时可能会报错

```text
* What went wrong:
Could not determine the dependencies of task ':app:compileDebugJavaWithJavac'.
> Could not create service of type AnnotationProcessorDetector using JavaGradleScopeServices.createAnnotationProcessorDetector().
```

这种情况基本下面的命令都可以处理：

```bash
flutter clean
```

### 自动生成类文件

可以参考\([https://www.jianshu.com/p/b307a377c5e8](https://www.jianshu.com/p/b307a377c5e8)\)这个文章处理下，当然类的属性没必要设置为final，当然如果你对于Dart的类比较困惑的话，可以参考下这个文章 [https://www.jianshu.com/p/9e31a9196981](https://www.jianshu.com/p/9e31a9196981)。

### 安卓APK打包

混合开发方不方便，看打包方不方便，反正flutter是很方面，ios还没有打包，暂时只打包了android的APK，这里说一下：

1. mashu\android\app\src\main\res目录下的应用图标换成自己的；
2. mashu\android\app\src\main\AndroidManifest.xml文件中配置下网络权限，不然你打的包无法发出http请求；
3. APK打包是要配置加密的，我不太清楚是干嘛的，但是是要的，按照网上找的教程跑一下。

### 

![&#x7801;&#x53D4;](../.gitbook/assets/gh_f4410a11c277_258.jpg)

