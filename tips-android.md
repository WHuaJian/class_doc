# Android

## 一、adb常用命令

### 1、查询已连接设备/模拟器

```
adb devices
```

### 2、指定设备获取屏幕分辨率

```
adb -s 设备号 shell wm size
```

### 3、给指定设备安装应用

```
adb -s 设备号 install test.apk
```

### 4、启动 adb server 命令

```
adb start-server
```

### 5、停止 adb server 命令

```
adb kill-server
```

### 6、查看adb版本

```
adb version
```

### 7、指定 adb server 的网络端口

```
adb -P <port> start-server
```

### 8、通过 IP 地址连接设备

```
adb connect <device-ip-address>
```

### 9、断开无线连接

```
adb disconnect <device-ip-address>
```

### 10、查看所有应用

```
adb shell pm list packages
```

### 11、卸载应用

<packagename> 表示应用的包名，-k 参数可选，表示卸载应用但保留数据和缓存目录。

```
adb uninstall [-k] <packagename>
```

### 12、查看设备型号

```
adb shell getprop ro.product.model
```

### 13、查看进程

```
adb shell ps
```



## 二、jCenter构建

### 1、构建命令

```
./gradlew clean build bintrayUpload -PbintrayUser=whj  -PbintrayKey=dc8d7364090002d050eb0a9da4a820b84e87f6f3  -PdryRun=false
或者
./gradlew bintrayUpload -PbintrayUser=whj -PbintrayKey=dc8d7364090002d050eb0a9da4a820b84e87f6f3 -PdryRun=false
```

