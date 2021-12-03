---
title: "Typora图片自动上传"
date: 2021-12-03T21:34:42+08:00
draft: false
---

# Typora图片自动上传

## 前言

### 现存问题

习惯使用markdown的人应该都Typora非常简洁高效，但导入文件的时候，只会把文件保存在本地计算机，文件中保存的是本地的绝对路径，当在本地写好文档上传当各个技术博客平台时，还需要手动的上传一遍，图片多的时候无疑是一个很大的工作量。

### 解决方案

Typora 编辑器在新版本上(≥ 0.9.9.32 on macOS or 0.9.84 on Windows / Linux) 增加了一个 `上传图片` 的功能，通过第三方app或脚本在插入图片时将图片上传到网络，然后文档中会直接插入图片的图床链接。

接下来介绍Typora如何通过PicGo来自动/手动上传图片到自建github图床。

> 图床就是一个你在网络上存放图片的地方，很多公司都提供了这样一个地方让你存放图片，比如微博，简书，七牛云等，但这些毕竟是公司资源，说不定哪天就不能访问了。所以，自己构建一个图床就显得比较重要了。

## 创建图床

GitHub + PicGo + jsDelivr 的方式是目前比较主流的方式。

1. GitHub 用来存放图片，目前投入微软怀抱，后台硬，不会随便倒闭
2. [PicGo ](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FMolunerfinn%2FPicGo) 是一个用于快速上传图片并获取图片 URL 链接的工具，通过该链接访问网络图片资源
3. [jsDelivr](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.jsdelivr.com%2F) 是用于开源项目的免费公共CDN(Content Delivery Network，即内容分发网络)，用来加速GitHub的访问速度

### 创建GitHub repository

![](https://i.loli.net/2021/12/03/5qlyzFbGD1UuLHd.png)

### 生成GitHub Token

进入个人设置 `Settings -> Developer settings -> Personal access tokens -> Generate new token`

![](https://i.loli.net/2021/12/03/ED9LkWAhzQgGpsF.png)

在`Note`中填好描述，z选择一个合适的过期时候后，勾选`repo`，然后点击`Generate token`生成一个Token

![](https://i.loli.net/2021/12/03/Zcp3yXUHJC78LaG.png)

**注意这个Token只会显示一次，所以，需要保存下来**

![](https://i.loli.net/2021/12/03/P6meLY5Fsr4XBGi.png)

### 配置PicGo

下载安装[PicGo](https://github.com/Molunerfinn/PicGo/releases)后，打开GitHub图床配置窗口

![](https://i.loli.net/2021/12/03/IGYVC2NwjSD9Ovc.png)

* 设定仓库名：按照 `用户名/图床仓库名` 的格式填写

  ```
  eg: fxding2019/PictureBed
  ```

* 设定分支名：`master`

* 设定Token：填写之前在GitHub生成的Token

* 指定存储路径：填写想要储存的路径，如 `BlogImg/`，这样就会在仓库下创建一个名为`BlogImg` 的文件夹，图片将会储存在此文件夹中。

  ```
  注意：BlogImg/ 不要写成 BlogImg
  ```

* 设定自定义域名：图片上传后，PicGo 会按照 `自定义域名+储存路径+上传的图片名` 的方式生成访问链接，并放到粘贴板上

  ```
  eg: https://cdn.jsdelivr.net/gh/fxding2019/PictureBed
  ```

  因为我们要使用 jsDelivr 加速访问，所以自定义域名可以设置为 `https://cdn.jsdelivr.net/gh/用户名/图床仓库名`，上传完毕后，就可以通过 `https://cdn.jsdelivr.net/gh/用户名/图床仓库名/储存路径/上传的图片名` 加速访问我们的图片了。存储路径为`BlogImg/`,  所以通过`https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/上传的图片名` 来加速访问。

* 点击确定以及设置为默认图床

* 打开`时间戳重命名`

  上传图片时，如果多次上传同一张图片的话，文件名会一样，导致文件已存在而报错，在上传图片时使用时间戳不会因为文件名重复而报错。

  ![](https://cdn.jsdelivr.net/gh/fxding2019/PictureBed/BlogImg/202112032215151.png)

* 测试上传

  拖动图片进入上传区后自动上传图片，并在github repository中自动创建目录存放图片

  ![](https://cdn.jsdelivr.net/gh/fxding2019/PictureBed/BlogImg/202112032217745.png)

## Typora设置自动上传

进入`文件 -> 偏好设置 -> 图片`，在`插入图片时`区域，下拉框选择`上传图片`，勾选如下三项

![](https://cdn.jsdelivr.net/gh/fxding2019/PictureBed/BlogImg/202112032221254.png)

点击`验证图片上传选项`测试上传效果

![](https://cdn.jsdelivr.net/gh/fxding2019/PictureBed/BlogImg/202112032220934.png)

至此，直接把图片粘贴到Typora时，就会转换为github图床的路径。