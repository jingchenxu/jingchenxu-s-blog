## nginx负载均衡，tomcat共享session

- 背景

在一些特定的项目背景下，要求系统24小时不宕机，但是期间还要能够上线一些新的功能，做到热更新，又或是项目的并发较高，需要进行负载均衡时，其实如果是简单的负载均衡，nginx已经为我们提供的丰富的方式。

- nginx负载均衡基本配置

首先说一种没有贡献session的负载均衡的方式，我们可以指定来自同一客户端的用户每次访问的tomcat为固定tomcat，具体的配置如下：

````config
upstream backserver { 
ip_hash; 
server localhost:8080; 
server localhost:8081; 
} 
````

以上的方式存在一定的问题，只能说是实现了最基本的负载均衡，用户一开始访问的是8080端口，那么他的session就是存储在8080端口的这台tomcat当中的，如果你这是关闭这台tomcat，nginx会自动的指向8081端口，但是问题来了，tomcat默认是将session存储在内存当中的，tomcat一旦关闭，那么他的session就消失了，这就是为什么我们本地开发的时候，tomcat重启后，客户端需要重新登录，采用这种负载均衡的方式就会出现这样的问题，任意一台tomcat下线，都有可能导致session丢失。

- redis存储session

上一种方案最后的问题在于session是存储在内存当中的，另外一个tomcat无法获取当前tomcat的session，那我们就要解决这个问题，那就是将session放在一个俩个tomcat都可以访问的地方，都可访问的地方是哪里呢，可以是一个文件，可以是一数据库，最重要的是2个tomcat都能够访问这个地方，这其实就是相当于重写，tomcat的session的写入和读出方法，原本是写入内存的，我们写入文件或数据库。

我们暂且先不讨论如何使用写入文件来实现这个功能，网上比较通用的方式是写入redis，那最重要的一点，如何实现tomcat将session写入redis，和从redis读出。

1. 安装redis

先创建一个文件夹，文件夹最好不要中文的，然后进入到这个文件夹下开始操作，执行下面的命令：

````shell
$ wget http://download.redis.io/releases/redis-3.2.1.tar.gz
$ tar xzf redis-3.2.1.tar.gz
$ cd redis-3.2.1
$ make
````

在以上命令执行完成后，我们需要启动redis，在执行完之前的命令的时候，我们应该是在解压目录下的：

````shell
root@ubuntu:/jinhetech/redis-3.2.1# ls
00-RELEASENOTES  CONTRIBUTING  deps     Makefile   README.md   runtest          runtest-sentinel  src    utils
BUGS             COPYING       INSTALL  MANIFESTO  redis.conf  runtest-cluster  sentinel.conf     tests
````

大致的我们可以看到在解压目录下的文件主要有这些，更多的文件是在src文件夹下，通过以下命令启动redis：

````shell
root@ubuntu:/jinhetech/redis-3.2.1# ./src/redis-server ./redis.conf
````

启动成功的场景是这样的，注意不要管理命令行窗口，因为这个命令不是后台运行的命令，如果你要继续进行操作，请启动另外的窗口：

````shell
12585:M 10 Sep 22:25:29.166 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.2.1 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 12585
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

12585:M 10 Sep 22:25:29.167 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
12585:M 10 Sep 22:25:29.167 # Server started, Redis version 3.2.1
12585:M 10 Sep 22:25:29.167 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
12585:M 10 Sep 22:25:29.167 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
12585:M 10 Sep 22:25:29.167 * The server is now ready to accept connections on port 6379
````
2. 配置redis

我们为了实现session存储到redis的功能，主要设置的有2个地方，一个就是设置redis的访问密码，还有一个就是开启redis的运程访问。

在解压的根目录下我们应该能找到redis.conf文件，编辑该文件，修改以上2个配置项，搜索文档中的bind 127.0.0.1 并注释掉；就算这个注释掉了也不意味着你就可以访问数据库了，你还需要禁用保护模式，搜索protected-mode yes,修改yes为no；最后设置一下密码，文档搜索requirepass foobared,设置requirepass 123456,以上配置完成后，记得用上面的命令将redis重启一下。

需要注意的是，如果你是通过命令创建的密码，在redis重启之后是会失效的，防止密码失效的方法是在redis的配置文件redis.conf文件中配置密码：

通过命令行配置代码如下：
````shell
./src/redis-cli
config set requirepass 123456
config get requirepass
//此时你想要获取密码的时候回提示你先登录
auth 123456
````

此外在redis的配置文件中默认的redis模式台启动的，所以我们可以看到上面redis启动的提示，一旦我们关闭当前的命令窗口，那么该redis也就关闭了，所以我们需要将redis设置为后台运行，在redis的配置文件当中，我们找到daemonize 这个选项，设置为yes即可。

3. 配置tomcat

接下来我们要实现的功能就是，tomcat的session读写目标为redis，而不是内存，我们会使用[tomcat-redis-session-manager](https://github.com/jcoleman/tomcat-redis-session-manager)这样的库来实现redis读写session，原本我们是需要，通过编译源码来获得实现此功能的jar包的，但是现在我们可以通过别人打好的jar包实现此功能（[下载地址](https://pan.baidu.com/s/1bokMOVH)），将里面的3个jar包放到tomcat的lib文件夹中，此外还需设置tomcat根目录下的/conf/context.xml文件，添加代码如下：

````xml
    <Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />        
    <Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager" 
        host="localhost"       
        port="6379"                 
        password="123456"            
        database="0"                 
        maxInactiveInterval="60" />
````

如果你正在部署你的项目，你可以将你的项目发布到tomcat当中，访问你的应用后，通过redis命令或是使用RedisDesktopManager客户端访问redis，查看session是否已被写入到redis数据库中，如果你只是想测试的话，你可以使用我的[测试项目](https://github.com/jingchenxu/drools-lesson)，我将会在该项目中添加一些示例。

4. 对象序列化

我们存储到session当中的数据可能并不都是java的基本数据类型，可能是一个java对象，那么对象如何存储到数据库当中呢！你可以搜索我的博客，查找java对象序列化相关的文章，tomcat-redis-session-manager支持将java对象序列化后存储到redis数据库，但是前提是我们的对象支持序列化，所以我们的类需要实现Serializable接口。




