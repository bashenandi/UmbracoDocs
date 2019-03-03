# 介绍 #

"创建一个基本网站"指南的以下部分，提供了一个 Umbraco 网站从简单的 HTML、CSS 和 JavaScript 文件开始构建的分布说明。它允许您将任何网站"模板"（例如简单的HTML、CSS 以及 JavaScript）安装到新的 Umbraco 中以及如何在 Umbraco 中将您需要的内容管理起来。

# **你需要准备的** #
要演示在 Umbraco 中创建一个基本站点，您需要以下内容：

*    一个干净的，空白的 Umbraco 初始化安装 – 比如不能安装新手网站包，查看下面在运行安装向导时的注意事项。使用最新版的 7.x 下载。跟着[安装文档](/)的步骤操作。
*    这个指导教程使用了一个初始包 - 一个 HTML5，响应式网站模板[https://github.com/verekia/initializr-template/archive/master.zip](https://github.com/verekia/initializr-template/archive/master.zip)，或者你自己准备了，也可以使用你自己的平面 HTML 文件。

# **入门** #

## 安装一个空白的 Umbraco ##

This guide doesn’t cover the installation of Umbraco – follow the instructions in https://our.umbraco.com/documentation/Installation. When you see the first splash screen click **_customize_** – either fill in your MS SQL blank DB credentials or use the CE option – then on the final screen use the “**_No thanks I do not want to use a starter website_**”. 


本指南不会包含完整的安装步骤 - 具体请前往[安装文档](/)查看。当您看到第一个启动画面时，点击  **_customize_** ，然后填入你的MS SQL 空白数据库认证信息或者选择 CE 版本数据库，在最后的画面中点击 “**_No thanks I do not want to use a starter website_**”。
 
![Umbraco 安装浮窗](images/figure-1-installation-splash-screen.png)


*图 1 - 安装启动画面 - 注意自定义链接*

![安装新手包](images/figure-2-no-starter-website.png)

*图 2 - 安装新手工具包 - 不谢谢!*


## 检查确保你安装的是空白 Umbraco ##

当你访问你的主机地址（http://localhost 或者你设置的任意地址），你会看到 Umbraco 空白页面界面。

 
![这是正确的 - 我们有一个空白的 Umbraco 网站](images/figure-3-empty-umbraco-install.png)


*图 3 - 这是正确的 – 我们有了一个空白的Umbraco 网站!*

如果你看到了 Umbraco 新手包网站，表示你错过了**_No thanks I do not want to use a starter website_**选项。  

 
![你不该看到这个!](images/figure-4-should-not-see-this.png)

*图 4- 你不该看到这个画面!*


如果你看到了这个新手包，你需要重新安装 Umbraco - 如果你是手工安装的，你需要删除本地主机网站服务目录中的所有文件，复制 Umbraco 压缩包内容备份到这里，然后再次在浏览器中访问 localhost 地址。


## 准备初始化模板网站 ##

现在解压缩初始包，把内容放在桌面的某个目录中（或者你选择的其他地方）。现在从这个目录中打开 **_index.html_** 到你的默认浏览器中会看到这个模板 - 你能看到一些填充了友好信息的文本以及假的链接。我们要把它转换一个Umbraco 管理的网站！

 
![初始模板](images/figure-5-initializr-template.png)

*图 5 - 初始化模板*


登录到 Umbraco（在浏览器输入http://localhost/umbraco）。你会看到一个空白脸的 Umbraco - 我们该如何开始！？
 
![没有内容的, 空白 Umbraco 安装](images/figure-6-umbraco-empty.png)

*图 6 - 没有内容的空白 Umbraco*


## 下一步 - [创建你的第一个文档类型](Document-Types.md) ##
如何创建文档类型和它们能做什么。

