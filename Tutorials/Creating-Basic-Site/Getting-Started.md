# 介绍 
跟随"创建基础网站"教程提供的一步一步介绍信息，从静态的 HTML，CSS 和 JS 文件开始构建你的 Umbraco 网站。它可以让你将任何网站"模板"安装到新的 Umbraco 中以及如何在 Umbraco 管理你需要的内容。 

# **你需要准备的**

通过 demo 在 Umbraco 创建一个网站，你需要先准备以下内容：

*    一个清洁的，空白的 Umbraco 安装 – 比如不能安装新手网站包，查看下面的内容注意在运行安装向导时要怎么做。使用最新版的 7.x 下载包。跟着[安装文档](/)操作。
*    这个指导教程使用了一个初始包 - 一个 HTML5，响应式网站模板[https://github.com/verekia/initializr-template/archive/master.zip](https://github.com/verekia/initializr-template/archive/master.zip)，或者你自己有，可以使用你自己的静态 HTML 文件。

# **入门**

## 安装一个空白的 Umbraco

这个指导不会包含完整的安装步骤 - 具体请前往[安装文档](/)查看。当您看到第一个浮窗时，点击  **_customize_** ，然后填入你的MS SQL 空白数据库认证信息或者选择 CE 版本数据库，在最后的屏幕中点击 “**_No thanks I do not want to use a starter website_**”。
 
![Umbraco 安装浮窗](images/figure-1-installation-splash-screen.png)


*图 1 - 安装浮窗 - 注意定制链接*

![安装新手包](images/figure-2-no-starter-website.png)

*图 2 - Install a starter website - No Thanks!*



## 检查你安装的是空白 Umbraco

当你访问你的主机地址（http://localhost 或者你设置的任意地址），你会看到 Umbraco 空白页面界面。

 
![这是正确的 - 我们有一个空白的 Umbraco 网站](images/figure-3-empty-umbraco-install.png)


*图 3 - 这是正确的 – 我们有了一个空白的Umbraco 网站!*

如果你看到了 Umbraco 新手包网站，表示你错过了**_No thanks I do not want to use a starter website_**选项。  

 
![你不该看到这个!](images/figure-4-should-not-see-this.png)


*图 4- 你不该看到这个画面!*


如果你看到了这个新手包，你需要重新安装 Umbraco - 如果你是手工安装的，你需要删除本地主机网站服务目录中的所有文件，复制 Umbraco 压缩包内容备份到这里，然后再次在浏览器中访问 localhost 地址。


## 准备初始的模板网站

现在解压缩初始包，把内容放在桌面的某个目录中（或者你选择的其他地方）。现在从这个目录中打开 **_index.html_** 到你的默认浏览器中会看到这个模板 - 你能看到一些填充了友好信息的文本以及假的链接。我们要把它变为一个Umbraco 管理的网站！

 
![初始模板](images/figure-5-initializr-template.png)


*图 5 - 初始化模板*

登录到 Umbraco（在浏览器键入http://localhost/umbraco）。你会看到一个空白脸的 Umbraco - 我们该如何开始！？
 
![没有内容的, 空白 Umbraco 安装](images/figure-6-umbraco-empty.png)


*图 6 - 没有内容的空白 Umbraco*

---

## 下一步 - [创建你的第一个文档类型](Document-Types.md)
如何创建文档类型以及它们做什么。

