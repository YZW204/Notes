## 错误

**1. his failure was cached in the local repository and resolution is not reattempted until the update interval of aliyunmaven has elapsed or updates are forced**

分析：

 ```
 Maven默认会使用本地缓存的库来编译工程，对于上次下载失败的库，maven会在~/.m2/repository/<group>/<artifact>/<version>/目录下创建xxx.lastUpdated文件。
 
 一旦这个文件存在，那么在直到下一次nexus更新之前都不会更新这个依赖库。
 
 ```

解决：

```
mvn clean package -U
```

> -U参数会强制update本地的jar（不用再专门去删除）

