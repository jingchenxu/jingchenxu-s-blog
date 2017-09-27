## win10下docker切换虚拟磁盘存储位置

自从开始使用了docker以后，发现自己的C盘越来越小了，估计也是默认的docker资源被安装到C盘了，docker在在Windows上使用的虚拟机为Hyper-V虚拟机，在启动docker的时候，docker会在Hyper-V当中创建一个虚拟机，该虚拟机会加载docker的虚拟硬盘，如果在默认目录下没有找到虚拟硬盘文件时，docker会主动创建一个虚拟硬盘，所以我们需要改变的是这个虚拟硬盘的位置，该虚拟硬盘的默认位置是在C盘的，这就是为什么最近C盘变大了很多的原因，我们需要改变虚拟硬盘的位置，docker创建的虚拟机的名称应该是MobyLinuxVM,选中该虚拟机，点击Hyper-V设置，可设置虚拟硬盘所在的位置，如下图所示：

![](/img/docker/docker02.png)

设置完成后，需重启docker生效，打开cmd查看docker的虚拟硬盘是否加载正确。可能你会出现以下问题：

````bash
error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.32/images/json: open //./pipe/docker_engine: Access is denied. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
````

该问题可能是因为你的cmd窗口不是已系统管理员的身份打开的。