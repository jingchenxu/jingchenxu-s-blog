# 单页应用中微信授权页面遇到的坑

公司用react框架开发的单页应用，在实际的使用过程中遇到了一个问题，我开发的是微信端的web应用程序，遇到的一个问题是在，应用中有多个页面需要进行支付，而在微信的后台设置中，微信只可以设置3个支付页面的url，这就导致了设置的支付路径完全不够用的问题。

先说一下解决方案，我也是参照了[微信支付授权单页应用解决方案](http://blog.csdn.net/liufeng520/article/details/51354741),这个可能不是原文，解决方案最好的应该就是这个了，很多人都进行了转载。

他的方案是，通过将单页应用中的路由后的字段作为参数而避免微信对地址的解析，示例如下：

> http://192.168.0.104:8080/index.html#/page1

我们在地址的‘#’号前添加了俩个问好，这样微信浏览器就会将问号后面的值全部解析为参数，则修改后的URL为：

> http://192.168.0.104:8080/index.html??#/page1

可能在网上的博客中广泛流传的方案是添加一个‘？’号就可以了，经过我个人的测试时不可以的，我前台采用的是react-router，网上博客中采用的是angular的路由不知道会不会有区别，经过测试发现在文件index.html与#号之间添加的任何的字符都是可以的，不会影响到实际访问的页面。

其实如果你是单纯的只是在安卓机的微信中进行开发，就我的项目来说是没有问题的，因为我的项目中只有2个涉及到支付，问题就出在iOS端的微信中对路由的解析方式是不一样的，在项目中分别有俩个地涉及到支付。

如果你的项目目录是这样的：

```
|--DEMO--|--folderA
         |--folderb
         |--folderc
         |--index.html(单页应用访问页面)--|--#/page1
                                            |--#/page2
                                            |--#/page3
                                            |--#/page4（假定此页面为支付页面）
```
那么你设置支付授权目录为.../DEMO，对于iOS来说，在进行支付时，检测到的当前路径为：.../DEMO,但是在安卓机下检测到的当前路径为：.../DEMO/#/page4,在安卓机的环境下“支付败北”。

生命不止，BUG不休，在dva框架下解决该问题的办法：

最近抛弃了自己搭建的前端工程化框架，采用的DVA框架，但是在支付路径上又出现了问题，如果通过location.href进行跳转时，发现了一个问题，那就是如果你跳转的地址为：```...DEMO/??#/page4```,则会自动跳转到```...DEMO/#/page4```，并且伴随着整个页面的重新刷新，可以猜猜我怎么办的？

故技重施```...DEMO/???#/page4```,但是在从```...DEMO/#/page3```跳转到```...DEMO/???#/page4```，整个页面都会重新刷新，解决该问题的方法，就是将所有的页面的地址都加上？？？

- 你可能还需要

[nginx配置history路由](./nginx-pei-zhi-xiang-guan/nginx-pei-zhi-history-lu-you.md)