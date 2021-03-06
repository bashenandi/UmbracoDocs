# 常规升级 #

_本篇常规的升级手册。有时这些准则会有一些例外情况，具体请参看 **[特殊版本文档](version-specific.md)**._

## 警告 ##

**重要**: 如果你要升级到一个全新的版本，例如从 Umbraco7 到 Umbraco8，确保您已经浏览过了[特殊版本文档](version-specific.md)。

不管出于任何原因，都可能导致过程出错。确保你**一直**拥有网站和数据库的备份，这样您就可以还原到你确知可以运行的版本。稍后你将需要这些备份文件来合并，因此这个步骤是不可缺少的。

在升级到一个新的主要版本之前（例如 v6到 v7），请检查你要升级到的版本是兼容的。在软件包下载页面， **项目兼容性** 部分，点击 **查看详细** 检查特殊版本的兼容性。

## 注意 ##
你需要在你的每一个 Umbraco 站点中运行升级安装程序。因此如果你想要同时升级测试和生产站点，你需要重复下面的步骤，确保在安装画面中点击并完成了更新操作。


## 使用 NuGet 安装? ##

你可以打开 **程序包控制台** 输入：
`Update-Package UmbracoCms`

系统会提示您确认文件会被覆盖，你应该选择 **"No to All"** 在点击 **"L"**。如果你升级到的版本需要任何特定的配置更改，那么它们将会在 **[特殊版本指南](version-specific.md)** 中被提及。

或者你可以打开 **NuGet Package Manager** 之后选择 **Updates** 面板来获取所有的更新版本。选择 **Umbraco CMS** 然后点击 **Update**。这样开始运行所有的更新程序，在确保更新到最新文件的同时，会留下你修改过的文件。*(这一段的翻译很别扭)*


#### 升级版本低于 7.2.0 ####
如果你未升级到7.2.0或者更高，你需要跟随下面的扩展说明进行操作，如果你更新至7.2.0以上，你可以跳过本段内容前往 [合并 UI.xml and language files](#MergeUIxmlandlanguagefiles)

**接下来的内容仅应用于更新版本低于7.2.0**  

你会被问到要覆盖你的 web.config 以及/config 目录中的文件，确保对这些问题回答 **No** 。

由于某些莫名其妙的原因，当被问到『File 'Web.config' already exists in project 'MySite'. Do you want to overwrite it?』，如果你点击『No to All』（在GUI中）或者回答『L』（在控制台中）可能安装程序会出错。所有确保你回答的是 "**No**" (在GUI中) or "**N**" (在控制台中).

![](images/nuget-overwrite-dialog.png)
![](images/nuget-upgrade-overwrite.png)

现在会出现一些棘手的问题：我们必须要覆盖你的 web.config 文件。但是我们会对其进行备份请勿担心。 (或者你也可以自行备份该文件) 你可以在 `App_Data\NuGetBackup\20140320-165450\` 中看到备份文件。 (目录 `20140320-165450` 会在备份时自动创建，随着日期时间的不同而变化) 接下来你可以合并这些配置文件，同时确保它们完全是最新的。

## 从 zip 文件包中手动升级 ##

从 [http://our.umbraco.org/download](http://our.umbraco.org/download) 下载你准备要升级的新版本.zip 文件。

从.zip 文件中复制下面的目录，并粘贴到你网站的同名目录中:

- /bin
- /Umbraco 
- /Umbraco_Client

**注意:** 有主机提供商（已知有 RackSpace Cloud）要求适当的文件和文件夹大小写命名。一般来说在 Windows 上这不会有问题，但是如果你的主体提供商强制要求适当的文件和文件夹大小写命名，你需要检查验证这些内容并确保新旧版本一致。

## 合并配置文件 ##
你可以对以下的配置文件进行一些修改:

* /Config 目录的任何文件
* /Global.asax 文件
* 网站根目录下的 web.config 文件 **(重要提示: 确保复制的版本号和数据库连接字符串一致)**
* /Views 目录下的 Web.config 文件


使用类似 [WinMerge](http://winmerge.org/ "WinMerge") 这类的工具检查所有配置文件之间的变化。取决于你上次更新时间，可能会有相当多的内容更新。

还有一种可能性，就是/Config 文件夹中的一些文件是新的，或者有些已经被删除了（我们在发布说明中做了标注）。WinMerge（或其他类似工具）能够比较文件夹，这样就可以更容易地发现这些差异。

直到版本6.0.0版本，升级时都有必要更改ClientDependency.config的版本号。这样可以显示的刷新后台缓存的HTML、CSS、JS文件。只需将版本号更改为高于当前版本号即可。不要省略这一步，否则后台可能会出现一些奇怪的问题。


## 合并 UI.xml and language files ##
一些软件包（例如 Contour）会添加对话框到 UI.xml 中。请务必在升级过程中，将这些文件从备份中合并到更新中，以便包继续正常工作。这些文件可以在：**/Umbraco/Config/Create/UI.xml** 找到。

一些软件包（例如 Contour 和 Courier）还会更改一些语言文件：/Umbraco/Config/Lang/*.xml (例如 en.xml)，不要忘记合并更新。

# 最终  #
当你复制文件和更新配置文件后，再次打开你的网站。你会看到安装程序，它将引导你完成升级。

安装器会做两件事：

* 更新版本号到 Web.config 文件
* 如果数据库有任何变化都会进行升级

安装程序在升级时会要求你提供详细的 **空白 database** 数据库信息。不久之后，这将会预先载入你已有的信息，所以不必在意。填写你已有数据库的相信信息，Umbraco 会在必要时予以更新。

## 安装之后 ##
一个非常重要的建议是在安装完成后删除 `install` 文件夹，并且绝对不要把他上传到生产服务器。

## 潜在的问题和陷阱  ##

### 浏览器缓存 ###
Google Chrome 大量的使用缓存内容，所以如果后台工作的不大正常，请确保浏览器缓存和cookie 已经彻底清除（其他浏览器也一样）。通常来说，浏览器缓存问题是由 Umbraco 获知 config/ClientDependency.config 版本号来处理的，如果你想要重置这些更新你可以增加这些版本变化来使 JS 个样式表以及服务器端缓存得以清除。

在 Chrome 中还有一个调整缓存的办法是打开开发者工具(F12)，然后前往设置(齿轮图标)。找到一个叫"Disable cache(while DevTools is open)"的复选框。一旦这个复选框打开，您再次刷新页面，缓存应该会失效。想要清理的更加彻底，可以右击地址栏右侧的"reload"按钮，显示菜单包括："Normal reload"、"Hard reload"和"Empty cache and hard reload"。最后一个选项是清理最彻底的，或许你想尝试一下。
