# 使用 NuGet 安装 Umbraco #

_跟着下面的步骤使用 NuGet  完成 Umbraco 的安装。_

## 摘要 ##

- 如果你在Visual SStudio **空白C# 网站应用**项目安装 Umbraco 会得到最好的结果
- 在 VS2012中：使用ASP.NET空网站应用
- 在 VS2013/2015/2017中：使用空模板的ASP.NET网站应用
- 在大多数情况下，Umbraco **不能**安装在现有的 MVC/Webforms 项目中
- 当问你是否要覆盖配置文件时，选择**"Yes"**来覆盖它们
- ReSharper 8可能会有一些问题，建议不要使用，或升级到ReSharper8.2.3

## ReSharper 用户请注意 ##
我们注意到 ReSharper 8的一个问题会导致在使用 NuGet 安装时发生错误（你可以投票帮助尽快解决这个问题[this issue][1]）。

建议你在安装 Umbraco 时，通过：**Tools > Options > ReSharper > Suspend Now** 来暂停 ReSharper 插件工作。

当你完成安装或者升级后，可以使用同样的方式再次启用。

## NuGet 版本 ##
开始之前：确保你的 NuGet 版本升级到最新。我们使用最新版本进行测试，并不保证同样的过程适用于旧版的 NuGet。

在 Visual Studio 中，前往 **Tools > Extensions and Updates**，然后选择 **Updates > Visual Studio** 目录。检查如果有 NuGet 的最新版本，请安装。

![](images/NuGet/nuget-update.png)

请同时确保**程序包管理器控制台**的执行策略为 **RemoteSigned**。你可以前往  **Tools > NuGet Package Manager > Package Manager Console**  进行检查。当控制台加载后，输入 `Get-ExecutionPolicy` ，如果它显示的是**Restricted**，请在控制台中输入后面的命令进行更改： `Set-ExecutionPolicy RemoteSigned`

## 在Visual Studio中安装 NuGet ##
 如果你还没有安装 NuGet，你可以在这里找到所有关于 NuGet 的信息：[http://docs.nuget.org/docs/start-here/installing-nuget](http://docs.nuget.org/docs/start-here/installing-nuget)。

## 新解决方案 ##
为了安装 Umbraco 我们首先需要一个Visual Studio解决方案

### Visual Studio 2012 ###
前往 **File > New Project** 并选择 ASP.NET Web 应用程序。点击 **OK** 并在下面选择：

* ASP.NET Empty Web Application
* ASP.NET Web Forms Application
* ASP.NET MVC 3 Web Application (将升级到 MVC 4 application)
* ASP.NET MVC 4 Web Application

MVC 选项应该还有第二步要选择，请选择其中的 **Empty** 模板项目。

有一点很重要，请务必选择上面列出的选项之一，而不要选择其他模板，或许会造成一些不可控的错误。

![](images/NuGet/new-project-vs2012.png)

### Visual Studio 2013/2015/2017 ###
打开 **File > New Project** 选择 ASP.NET  Web 应用程序。    

![](images/NuGet/new-project-vs2013-1.png)

在下一步中，选择 **Empty** 模板。选择**空**模板很重要，因为其他模板包含不兼容的MVC和JSON.NET版本。(不要勾选任何复选框来添加一些目录和引用。Umbraco 会为你添加)。

![](images/NuGet/new-project-vs2013-2.png)

或者你可以使用 VS2012/2013/2015 模板：

* ASP.NET 空网站模板

如果还有下个步骤，请选择 **Empty** 来防止冲突。

![](images/NuGet/new-project-vs2013-3.png)


## 查找并安装 Umbraco 程序包 ##
Umbraco 的最新发布版可以在 NuGet 目录中找到。你所要做的就是搜索并安装。

在Visual Studio 界面中安装 Umbraco,  右击前面创建的新项目，选择 **Manage NuGet Packages**。

![](images/NuGet/manage-nuget-packages.png)

你可以使用搜索功能找到名为 **Umbraco CMS** 的程序包。你同时可以看到 Umbraco CMS Core 程序包，当你选择安装Umbraco CMS 时，它将会被自动安装。确认选择了 Umbraco CMS (版本可能比下图要高) 然后点击 **Install**。

![](images/NuGet/nuget-search.png)

NuGet 接下来会下载依赖并且将所有的文件安装到你的解决方案中。

在这个过程中，会问到你是否要覆盖你的 web.config 文件。在本例中，由于我们是新创建的项目，所以覆盖是安全的。如果你在一个已存在的项目中安装 Umbraco，你可能想在回答 "Yes"之前，为已存在的 web.config 文件，创建一个备份文件。

![](images/NuGet/nuget-overwrite-dialog.png)

##程序包管理器控制台
你可以在程序包管理器控制台中做同样的事情，由于你不必点击菜单和搜索，反而会更快一点。

打开 **Tools >  Library Package Manager >  Package Manager Console** 启动控制台。

![](images/NuGet/enable-package-manager-console.png)

然后输入 `Install-Package UmbracoCms` 开始安装最新版本的 Umbraco。

![](images/NuGet/package-manager-console.png)

在这个过程中，会问到你是否要覆盖你的 web.config 文件。在本例中，由于我们是新创建的项目，所以覆盖是安全的。如果你在一个已存在的项目中安装 Umbraco，你可能想在回答 "Yes"之前，为已存在的 web.config 文件，创建一个备份文件。

![](images/NuGet/package-manager-console-overwrite.png)

## 运行网站 ##
现在你就可以像其他时候一样在中 Visual Studio 运行网站(使用 **F5** 或者 **Debug** 按钮)。

跟随安装向导，经过一些简单的步骤和选择，你会看到安装成功的提示信息。

## 安装之后 ##
当你使用 Visual Studio 的 **Web 一键发布**  时，Umbraco nuget 构建总会包含 Umbraco 文件夹。

你可以在 `packages/UmbracoCms x.y.z/build/UmbracoCms.targets`  中看到这些目录。

如果你想要排除这些目录和内容，你可以添加 **Target** 到  `properties/Publish` 目录中的 `.pubxml` 文件。在这个案例中，如果你需要在生产环境中排除 json data 插件，你可以配置：

	<Target Name="StopUmbracoFromPublishingAppPlugins" AfterTargets="AddUmbracoFilesToOutput">
		<ItemGroup>
			<FilesForPackagingFromProject Remove=".\App_Plugins\UmbracoForms\Data\**\*.*"/>
		</ItemGroup>
	</Target>

[1]: https://youtrack.jetbrains.com/issue/RSRP-419513
