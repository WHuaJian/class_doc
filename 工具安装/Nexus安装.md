# 安装

[下载地址]: https://www.sonatype.com/nexus/repository-oss/download

> 选择Unix版本



下载后使用命令解压

```
tar -zvxf 压缩包
```

> 解压后会有两个文件夹

<img src="E:\Code\LearnDemo\学习文档\运维\工具安装\images\image-20201009104741166.png" alt="image-20201009104741166" style="float:left;zoom:150%;" />



### 配置修改

修改文件位置`.nexus/bin/nexus`将`run_as_root=false`。

这样不会提示root启动



修改文件位置`.nexus/bin/nexus.vmoptions`。解除内存限制但最少也要2G内存

<img src="E:\Code\LearnDemo\学习文档\运维\工具安装\images\image-20201009105512578.png" alt="image-20201009105512578" style="float:left;zoom:150%;" />



### 启动Nexus

```
./nexus run &
```

> 默认端口8081