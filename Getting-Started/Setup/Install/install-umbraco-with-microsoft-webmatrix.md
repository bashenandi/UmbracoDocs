#通过Microsoft WebMatrix安装Umbraco 

按照这些简单的步骤，使用WebMatrix可以快速并且容易的运行你的实例。使用WebMatrix的好处就在于，它非常的快速以及简易。

##下载并启动 WebMatrix3

1. 前往 [http://www.microsoft.com/web/webmatrix/](http://www.microsoft.com/web/webmatrix/) 下载免费的 WebMatrix 3。

2. 正确安装后，启动WebMatrix。

3. 在显示的三个选项中，选择**New**。然后选择**App Gallery**。

  ![Web Matrix - Choose New](images/WebMatrix/webmatrix3-start.PNG)

4. 在右上方的搜索框内，输入"Umbraco"。

  ![Web Matrix - Search for Umbraco CMS](images/WebMatrix/webmatrix-search.png)

5. 在结果列表中选择**Umbraco CMS** 。点击**Next**。

6. 在**Site Name**输入框中给你的网站起一个适合的名字然后再次点击**Next**。

  *在搜索结果的下拉条目中，可能会比截图中包含的 Umbraco 项目要多。这是因为 WebMatrix 使用 WebPI 安装时会在后台下载一些其他软件来帮助网站运行。*

7. 在下一屏中你会看到 Umbraco 有两个可用的数据库选项。确定并选择其中的 **SQL CE Server (Installed)** 然后点击**Next**。这是 Umbraco 最简单快速的选择-对于大多数中小网站来说，使用存储在磁盘上的文件作为数据库是比较便利的方式。

  ![Web Matrix - Database Selection](images/WebMatrix/webmatrix3-database.png)

  *如果你需要将你的数据库从SQL CE Server迁移到完整版的SQL Server，你可以在之后的任意时间点进行。*

8. 点击 **I Accept** 接受授权协议和条款。

9. WebMatrix会自动下载运行 Umbraco 所需的文件。当完成时你会看到确认信息。

  ![Web Matrix - Install Complete](images/WebMatrix/webmatrix3-install-complete.png)

## Umbraco 安装网站
本节继续讲述的内容包含当你第一次运行网站时，需要安装和配置的内部信息。	
1. 当你完成安装后你可能需要从 WebMatrix 中启动网站 (WebMatrix也可能会在完成安装后自动启动网站。)
  1. 点击左下方的站点部分。

  2. 然后点击类似于 `http://localhost:22830`的 URL。

     *端口号由于是随机生成的，因此会与这里不同*。

  3. Umbraco网站安装程序会在你的默认浏览器中启动。

  ![Web Matrix - Finding Local Host](images/WebMatrix/webmatrix3-localhost.png)

2. 你会看到欢迎画面。阅读后，点击 **Lets get started!**

  ![Web Installer - Lets Get Started](images/WebMatrix/web-start.png)

3. 在下一屏，查看许可条款和条件，然后点击 **Accept and Continue**。

4. 在下一屏中你会看到 Umbraco 可以使用的4个数据库选项。选择**I want to use SQL CE 4** 后点击**Install**。

  ![Web Installer - Database choice](images/WebMatrix/web-db-CE.png)

  1. 在数据库安装期间你会看到进度条提醒，安装结束点击 **Next**

  ![Web Installer - Database Install](images/WebMatrix/web-db-install.png)

5. 在下一屏你需要在表单中创建可以在 Umbraco 后台中操作的用户。输入完成后点击 **Create user**

  ![Web Installer - Create User](images/WebMatrix/web-user.png)

6.  下一步，你可以选择是否安装 starter kit。Starter kit会安装一个示例网站，并允许你分析它，并学习 Umbraco 是如何运作的。

7. 在决定你是否要安装 starter kit 之后你就完成了整个安装过程！

8. 现在，点击 **Set up your new website** 登入后台。

  ![Web Installer - Install Complete](images/WebMatrix/web-finish.png)

9. 恭喜 - 你已经完成了!

### 祝贺你，已经安装了一个 Umbraco 网站!

### 注意
*你可以在浏览器中直接输入网址: http://yoursite.com/umbraco/进入后台*

###安装之后
一个非常重要的建议是在安装完成后删除 `install` 文件夹，并且绝对不要把他上传到生产服务器。