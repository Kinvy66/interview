# Typora+PicGo实现笔记图片上传

Typora+PicGo实现笔记中图片上传到服务器，避免Markdown文件拷贝时图片打不开的问题



## 软件工具

* 系统：Windows10（macOS请参考 [官方文档](https://support.typora.io/Upload-Image/)）

* [Typora](https://www.typora.io/)（版本：≥ 0.9.9.32 on macOS or 0.9.84 on Windows / Linux）

* PicGo（版本：≥ 2.2.0 ),在Typora软件下载（文件-->偏好设置-->图像）

  ![image-20210223162118642](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223162118642.png)

* 阿里云OSS对象，（使用gitee或GitHub也可以）



## 阿里云OSS的购买和使用

1. 购买阿里云OSS，我购买的是40GB一年的（9RMB）

2. 在控制台找到OSS管理，创建Bucket，读写权限设置为公共读，名称按照规定任意写一个，其他设置默认就可以

   ![image-20210223162911422](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223162911422.png)





## 配置PicGO

* 打开PicGo配置阿里云OSS

![image-20210223163326613](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223163326613.png)

> 设定KeyId：在阿里云的控制台中，如下图1，没有AccessKey ID 的话可以自己创建一个，然后将 AccessKey ID 复制道KeyId
>
> 设定KeySecret：在创建的AccessKey ID中查看Secret，把它复制到设定KeySecret
>
> 设定存储空间名：Bucket的名称
>
> 确认存储区域：阿里云控制台中，图2



图1：![image-20210223163225297](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223163225297.png)



图2：![image-20210223164008664](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223164008664.png)





* **配置PicGo的端口36677** 

  ![image-20210223164243108](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223164243108.png)

![image-20210223164315411](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223164315411.png)







## Typora设置

* 文件-->偏好设置-->图像，  设置如下图

![image-20210223164504527](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20210223164504527.png)



> 设置完毕，将图片添加到Markdown笔记中，会自动上传到服务器，图片链接也会转成一个URL







**注意：PicGo的端口一定要设置正确，否则图片会上传失败**