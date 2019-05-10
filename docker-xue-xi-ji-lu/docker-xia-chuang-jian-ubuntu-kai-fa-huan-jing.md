# docker下创建Ubuntu开发环境

> 最近看到vscode可以链接到docker容器内部直接进行开发，觉得这种PC下是用Ubuntu的方式很不错，就想尝试下，相比较win10下的linux子系统，可能还是这种方式好一点。

## 创建Ubuntu docker容器

1. 创建Ubuntu docker 容器

```bash
docker run -it -v F:\docker\jingchenxu:/jingchenxu -p 23:22 -p 5000:5000 -p 5001:5001 -p 5002:5002 -p 5003:5003 --name ubuntu ubuntu bash
```

上面的这条命令用于创建一个容器，该容器运行Ubuntu，-it是第一个参数，-t让docker分配一个伪终端并绑定到容器的标准输出上，-i则让容器的标准输入保持打开；`-v F:\docker\jingchenxu:/jingchenxu` 则表示将本地的 F:\docker\jingchenxu 文件夹映射到容器中的 /jingchenxu 文件夹当中；

