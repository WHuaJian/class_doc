# 课堂后台项目—CI/CD操作手册

## 1、登录Jenkins

```
地址：http://139.196.141.79:8888 
账号：admin
密码：85169336
```

![image-20201020164048007](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020164048007.png)

## 2、后端项目构建

### 2.1  构建步骤

![image-20201020164539775](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020164539775.png)



### 2.2 查看提交记录

![image-20201020164803602](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020164803602.png)

![image-20201020164831668](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020164831668.png)



## 3、前端项目构建

### 3.1 构建步骤

![image-20201020165024746](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020165024746.png)



### 3.2 查看提交记录

步骤同上



## 4、钉钉通知

每次构建成功后，会自动在项目组的钉钉群发布通知，注意查看！

![image-20201020172708115](/Users/weihuajian/Library/Application Support/typora-user-images/image-20201020172708115.png)



## 5、构建成功

访问地址：http://119.45.222.136/

也可在钉钉群通知信息点击“课堂管理后台”链接快捷进入。



## 6、注意事项

### 6.1 目前未分配多个账号，大家共用一个admin账号，尽量避免并发构建，导致服务器压力过大。

### 6.2 开发提交代码后自己构建或告知测试去构建，不要在代码无改动的情况下随意构建；

### 6.3 后期会分配每人对应的权限账号，避免交叉使用；



