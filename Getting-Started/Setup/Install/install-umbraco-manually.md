# 手动安装 Umbraco #

_跟着下面的步骤一步步完成 Umbraco 的手动安装。_

## 下载 Umbraco 二进制文件 ##
在 [our.umbraco.org/download](http://our.umbraco.org/download/) 上可以找到稳定版的二进制文件。如果你不介意使用实验性的构建，你可以下载 [夜版](http://nightly.umbraco.org/) 但是你需要自己承担风险 (并不保证它能正常运行)。

## 解压缩文件 ##
把你下载的二进制保存到磁盘中，请确保该文件没有被Windows安全机制阻拦。右键单击下载的文件并选择“属性”。如果在“属性”窗口的底部提示：『此文件来自另一台计算机，可能会被阻止以帮助您保护计算机』，然后请点击“解锁”按钮，之后二进制文件就可以被解压缩了。

现在你可以解压缩二进制文件到你选择的本地位置（例如： `D:\Dev\MyUmbracoSite\`)

## 选择你的 Web Server ##
有两种方式在服务器运行应用程序文件：

1. 简单的途径: 使用 IIS Express
2. Internet Information Server (IIS)

### 使用 IIS Express ###

你可以从 [Web Platform Installer (WebPI)](http://www.microsoft.com/web/downloads/platform.aspx) 上下载 IIS Express( 搜索关键词 "IIS Express")。如果要在 Visual Studio 2010中开发使用 IIS Express，你需要安装 Visual Studio 2010 Service Pack 1，这也可以在 WebPI 中找到。

![IIS Express on the Web Platform Installer](images/Manual/2012-03-17_164508.png)

现在，你就可以从命令行启动 IIS Express 并为你的网站提供服务了，在需要时使用Microsoft WebMatrix (也可以通过 WebPI) 启用 IIS Express 的实例会更加容易。 事实上，在WebPI 中安装 WebMatrix时，会自动下载 IIS Express 作为依赖。

WebMatrix一旦安装，你可以右击之前解压缩umbraco后的文件夹，从打开的菜单中选择“打开一个网站微软WebMatrix”从上下文菜单（应该在接近顶部的位置）。当 WebMatrix 开始运行，你就可以点击『Run』按钮启动你的网站。使用 WebMatrix 还有一个额外的好处：他可以允许你在网站运行时修改你的网站文件。

![Start Umbraco through Web Matrix](images/Manual/2012-03-17_173822.png)

### 使用 IIS ###
如果你想使用 IIS 为你的 Umbraco 网站提供服务，你可以使用主机名指向你的本地机器。

*注意: 我们强烈建议使用IIS7(.5). 或许 IIS6 可以工作, 但是本文档仅覆 IIS7。 IIS7 包含于Windows 7 和 Windows Server 2008.*
​	
在 IIS 中，右击**网站**，选择『添加网站……』创建一个新的站点。站点名称可以任意指定。
​	
接下来你一定要选择（或者创建）一个基于 ASP.Net 4.0的应用程序池 (默认的 ASP.NET 4.0 配置过于复杂，所以建议你选择另外一个)。 点击『选择……』按钮来选择创建好的应用程序池。你不必再做其他设置。

在物理路径中，填写之前Umbraco 解压缩后的文件夹。	

最后，填写主机名，在这里例子中我们使用『*MyUmbracoSite.local*』。
​	
![Configure new website in IIS](images/Manual/2012-03-12_223022.png)
​	
 最后一部，你需要添加 *MyUmbracoSite.local* 主机名到你的**hosts**文件。如果你没有更改过主机文件，请在那之前确保当前用户拥有写入 **hosts** 文件的权限。**Hosts**文件通常位于 C:\Windows\System32\drivers\etc\hosts (文件没有扩展名)。
​	
要使您能够写入**hosts**文件，右键单击它并单击“属性”。转到“安全”选项卡并找到当前登录的用户，单击用户，然后单击“编辑…”按钮。勾选*允许修改*权限。

*注意: **这是有风险的操作**。 如果你的账户可以修改 hosts 文件，恶意软件也可以利用你的账户访问，并试图更改主机文件将正常网站定向至未知的站点。为了您的安全，请务必在修改完成之后恢复安全设置。*
​	
在 hosts 文件中添加下面一行内容，将主机名指向到本地机器：

```
127.0.0.1 MyUmbracoSite.local
```

现在你前往 http://MyUmbracoSite.local 将会看到安装向导画面。

*请注意：你无法使用 Visual Studio 内置的 webserver 运行 Umbraco。不过如果你需要的话，可以设置 VS 使用 IIS Express 或者 IIS。*

为了使 Umbraco 有正确的权限可以写入磁盘，你需要为 IIS_IUSERS 账户设置可以修改之前解压缩 Umbraco目录修改的权限。

虽然为开发环境设置完整的权限是较好的方式，但是对于生产服务器你可能想要进一步限制权限。请浏览 [权限](permissions.md) 获取详细信息。

### 选择数据库环境 ###

关于数据库环境，有三个选项以供选择：

1. SQL Compact Edition
2. SQL Server 2008 (Express 或更高
3. MySQL 5 (或更高)

### SQL Compact Edition ###

如果希望快速开始，SQL CE 是一个很好的选择。你不需要提前在服务器端创建数据库，而且它是免费的。但同时意味着，对于海量内容的网站来说，它的扩展性不好。一但到达了SQL CE 性能瓶颈，您可将其迁移至SQL Server 数据库中。

如果要开始使用 SQL CE，只要在安装向导中选择：『*I want to use SQL CE 4, a free, quick-and-simple embedded database*』即可。

### SQL Server 2008 ###
如果你要使用 SQL Server ，在继续安装 Umbraco 之前，你需要在其中创建一个空的数据库。如何配置数据库完全取决于你的需要，但是要确保连接基于 TCP/IP 协议并且拥有 SQL 用户和密码（你也可以使用Windows身份验证，但这需要你自行编写连接字符串）。

一般来说，对于开发环境，你需要确保数据库用户拥有数据库所有者的权利，但一定要遵守你或你的工作环境为此设置的规则。

一旦你创建了数据库和凭据，在选择『*I already have a blank SQL Server 2008 database*』选项之后，请在安装向导中输入这些详细信息。

### 结尾 ###

按照安装向导进行操作，经过一些简单的步骤和选择，您应该看到安装成功的提示信息。