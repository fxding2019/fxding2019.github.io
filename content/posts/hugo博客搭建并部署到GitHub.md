---
title: "Hugo博客搭建并部署到GitHub"
date: 2021-12-03T20:58:43+08:00
draft: false
---

# hugo博客搭建并部署到GitHub【 for Windows】

[TOC]

### 下载配置hugo

1. 打开 Hugo 官方 GitHub 的 [**Releases**](https://github.com/gohugoio/hugo/releases) 页面
2. 选择最新的版本下载（选择 `hugo_xxx_Windows-64/32bit.zip`），如果使用`meme`主题，要下载extended版本的。
3. 将压缩包内的 `hugo.exe` 文件解压至 `D:\hugo` 文件夹目录下
4. 在 `文件资源管理器`（即 `我的电脑`）中空白处点击鼠标右键，打开属性
5. 依次点击 `高级系统设置` - `环境变量` ，双击打开系统变量中的 `Path`
6. 在环境变量界面中双击空白行，添加 `D:\hugo`，点击确定

打开命令提示符，输入语句：

```undefined
hugo version
```

以确认 Hugo 是否安装成功（如果安装成功就能看到版本号）

### 新建github repository

新建一个后缀为`.github.io`的 repository

![](https://i.loli.net/2021/12/03/FhYJUyq2nQVld91.png)

### github与hugo建立关联

1. 直接clone上述的repository【也可以自己在本地计算机建好文件，然后使用` git remote add origin`与远程仓库建立关联】

2. 进入repository的根目录，运行git bash here，执行以下命令

   ```
   hugo new site .   // 在当前目录下创建hugo站点
   ```

### 安装主题

进入到`themes目录`下，在[官网主题](https://themes.gohugo.io/)中选择一款主题，或使用本作者[推荐的主题](https://github.com/reuixiy/hugo-theme-meme)，`git clone`下来之后把`themes/meme/config-examples/zh-cn/`目录下的`config.toml`文件复制一份到repository根目录下，修改相关的配置，更多meme主题配置介绍见[这篇文章](https://io-oi.me/tech/documentation-of-hugo-theme-meme/#hugo)

```
// 默认为：
baseURL = "http://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"

// 修改为
baseURL = "github发布出去的博客地址"
languageCode = "en-us"
title = "My New Hugo Site"
publishDir = "docs"
```

修改了baseURL，并增加了publishDir。publishDir的修改是因为：当在content目录下添加了.md文件后，需要执行`hugo -t <主题名>`来用hugo把发布用的目录编译出来，而默认的编译目录名为`publish`，这里改成了`docs`，这是因为Github规定了用做Gitpages的分类只有三种：

1. 发布路径在master分支下。
2. 发布路径在master分支的docs目录下。
3. 发布路径在gh-pages 分支下。

只将默认的发布目录/publish推送上去，不利于跨设备修改我的博客，因此要将整个目录都推送上去，然后将/publish改成/docs，给github识别成gitpages用。

> 详情见  [User, Organization, and Project Pages](https://help.github.com/articles/user-organization-and-project-pages/)，参考自[Hugo配合GitHub搭建博客（Windows 10）](https://www.jianshu.com/p/02b3343295ac)

### 新建文章

1. 根目录下git bash执行

   ```
   hugo new "posts/hello-world.md"
   hugo new "about/_index.md"
   ```

2. 本地运行

   ```
   hugo server -D
   ```

   不到 100 ms 后，打开 http://localhost:1313/，恭喜你！你已经成功在本地搭建好博客了🎉🎉🍻！

   > 注意：如果不加`-D`参数，本地运行看不到相关文件内容，可以打开文件，把`draft=true`改为`false`，然后运行`hugo server`进行查看

### 发布文章

1. 把文件的`draft=true`改为`false`

2. 执行以下命令生成发布目录【经上面的配置之后，在docs下生成】

   ```
   hugo -t meme  // 根目录下执行，meme是hugo主题名
   ```

3. 使用git上传发布

   ```
   git add .
   git commit -m "XXX"
   git push
   ```

4. 在github repository的`settings -> Pages`中，把GitHub Pages Source改为`docs`目录，点击save保存

   ![](https://i.loli.net/2021/12/03/dvwHtoZPXph8miQ.png)

5. 点击published site连接即可访问

   > **注意**：如果本地更新之后，github published site页面内容没有更新，可能是浏览器缓存造成的，清理缓存即可