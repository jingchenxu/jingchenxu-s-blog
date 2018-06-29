# vue 后台管理类项目阶段性总结

> 之前的博客应该有说过了，公司打算尝试下是用vue搭建后台管理类系统的前端框架，目前大致已经完成了2个项目，但是目前项目还在开发当中，可能还会遇到新的问题，现就目前开发阶段的问题进行一个阶段总结。

### 项目结构

```
├─build                                          // webpack打包配置文件
├─config                                         // webpack各种运行模式的配置文件
├─dist                                           // 打包之后生成的文件
├─src                                            // 项目相关的源码所在位置
│  ├─assets                                      // vue-cli生成的没用到
│  ├─components                                  // 在整个项目中都会用到的通用组件
│  ├─iconfont                                    // 引入阿里图标弥补UI组件库自带图标不足的问题
│  ├─images                                      // 前端图片资源
│  ├─mixin                                       // 整个项目中通用的mixin
│  ├─router                                      // 前端路由
│  ├─store                                       // vuex
│  │  └─modules
│  ├─template                                    // 在项目中通用的模板页面
│  ├─utils                                       // 项目中通用的工具函数
│  │  └─vueplugin                                // vue插件
│  └─views                                       // 前端页面位置
│      └─mis                                     // 每个子项目在mis下创建一个文件夹
│          ├─components                          // 在子项目中通用的组件
│          ├─exception                           // 子项目异常跳转页面
│          ├─test                                // 子项目中用于测试的页面
│          └─workspace                           // 子项目模块
│              ├─components                      // 子项目模块组件
│              ├─config                          // 子项目模块配置参数文件
│              ├─mixin                           // 子项目模块mixin
│              └─mock                            // 子项目模块模拟说句
├─static                                         // 第三方静态资源
└─test                                           // 前端测试相关
    ├─e2e
    └─unit
```



