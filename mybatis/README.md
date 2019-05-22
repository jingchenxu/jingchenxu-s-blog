# mybatis

> 网上很多的人说mybatis的二级缓存比较鸡肋，一级缓存也没什么用，但是个人觉得其还是有用的。

## mybatis的缓存

mybatis的缓存分为一级缓存和二级缓存，一级缓存实在session内的，这个基本上没什么用，我们主要说二级缓存，二级缓存的实现方式是通过在内存中维护一个字典对象来实现的，内存中的读取速度自然是比数据库读取快的，当然还可以通过redis这样的内存数据库来管理mybatis的二级缓存，这也就解释了为什么能进行二级缓存的对象都需要实现序列化接口。

```markup
<cache eviction="LRU" flushInterval="10000" size="1024" readOnly="true"/>
```

配置mybatis缓存只需要在mapper文件当中设置以下的配置即可，每个配置的具体说明可以查看官网，如果数据库中的数据旨在一个系统中进行修改，且所有的数据都通过mybatis来操作，那么设置缓存是没有问题，但是如果数据可能在别的系统中发生了修改，这就导致了mybatis缓存中的数据与数据库中的数据不一致怎么办。

但是二级缓存是不是就没有用呢！正好在现在的系统当中就存在这样的使用场景，用户频繁的刷新列表数据怎么办，每次查询都可能和数据带较大的压力，这个时候开启一个缓存失效时间比较短的二级缓存就比较有用了，可以避免频繁刷新而各数据库服务器带来较大的压力。
