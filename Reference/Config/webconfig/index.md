# Web.config #

_本章定义了可以在 web.config中找到的 appsetting 参数_

## 必填的配置 ##
_web.config 文件中这部分 appSetting 的配置，必须要一个明确的值_

### umbracoConfigurationStatus ###
保持这个版本号为当前安装的 Umbraco 版本。这个版本号会在运行安装或者升级安装时自动更改。不建议手工更改这个值，升级安装需要在升级的网站时执行完一些动作后更改这个值。当这个版本号和Umbraco DLL的版本号一样时，升级安装器是不会运行的。

### umbracoReservedUrls ###
逗号分隔的 Umbraco 需要单独留下的文件。IIS 将保留这些文件，Umbraco 的请求管道不会触发这些文件。

    <add key="umbracoReservedUrls" value="~/config/splashes/booting.aspx,~/install/default.aspx,~/config/splashes/noNodes.aspx,~/VSEnterpriseHelper.axd" />

### umbracoReservedPaths ###
逗号分隔的 Umbraco 需要单独留下的文件夹。如果你有文件夹包含一些自定义文件，将其添加到这个设置中，Umbraco 会一直保留它们。

    <add key="umbracoReservedPaths" value="~/umbraco,~/install/" />

### umbracoPath
URL 指向 Umbraco 管理文件夹。如果你重命名了`umbraco`文件夹，你也需要更新这个设置。

    <add key="umbracoPath" value="~/umbraco" />

### umbracoHideTopLevelNodeFromPath ###
如果你运行了多个网站，你可能不想最高层级的节点显示在 url 中。可用的选项是`true`和`false`。

    <add key="umbracoHideTopLevelNodeFromPath" value="true" />

### umbracoUseDirectoryUrls

Strips `.aspx` from URLs on the frontend when set to `true`.  
This setting is only important to older IIS configurations where extension-less URLs weren't supported very well.

	<add key="umbracoUseDirectoryUrls" value="true" />
	
### umbracoTimeOutInMinutes ###
配置为当Umbraco 用户没有任何请求后的多少分钟，需要用户重新登录。任何后台请求都会重置这个时钟。默认设置是20分钟。

    <add key="umbracoTimeOutInMinutes" value="20" />

### umbracoDefaultUILanguage ###
如果没有为用户显式的指定语言时，登录后台默认显示的语言。默认是English(en)。

	<add key="umbracoDefaultUILanguage" value="es" />

### umbracoUseSSL ###
当设置为`true`时，要求所有后台的请求都使用 HTTPS 协议来替代 HTTP。
	
	<add key="umbracoUseSSL" value="true" />

### umbracoCssDirectory ###
通过添加这个值你可以指定一个新的文件夹用于存储你的 css 文件，并在可以在 Umbraco 中编辑它们。默认文件夹是：`~/css`。例如你想把 css 存储在网站根目录下面的"/assets/css"目录中，你可以修改这个appsetting ：

	<add key="umbracoCssDirectory" value="~/assets/css" />

### umbracoMediaPath ###
通过添加这个值你可以指定一个新的文件夹用于存储你的媒体文件。默认文件夹是：`~/media`。例如你想把媒体文件存储在网站根目录下面的"/umbracoMedia"目录中，你可以修改这个appsetting ：

	<add key="umbracoMediaPath" value="~/umbracoMedia" />

### umbracoScriptsPath ###

通过添加这个appsettings，您可以指定一个新的/不同的文件夹来存储您的javascript文件，并且仍然能够在umbraco中编辑它们。默认文件夹是~/scripts。例如，如果要将javascript存储在网站根文件夹“/assets/js”中的文件夹中，则可以编辑这个appsetting：

	<add key="umbracoScriptsPath" value="~/assets/js" />

### SMTP Settings ###

通过将此设置添加到web.config，将能够从Umbraco中发送电子邮件。这可能是工作流的通知邮件，或者使用表单的邮件，您需要指定SMTP设置才能使用电子邮件工作流。后台的忘记密码功能还需要一个SMTP服务器来发送带有重置链接的电子邮件。

By adding this settings to the web.config you will be able to send out emails from your Umbraco installation. This could be notifications emails if you are using content workflow, or you are using Umbraco Forms you also need to specify SMTP settings to be able use the email workflows. The forgot password function from the backoffice also needs a SMTP server to send the email with the reset link.

	<system.net>
        <mailSettings>
        <smtp from="noreply@example.com">
            <network host="127.0.0.1" userName="username" password="password" />
        </smtp>
        </mailSettings>
    </system.net>

## 选填配置 settings

_这些设置有默认值，但是能够通过在 web.config 中创建新的 appSetting 并设置它们的值来重写_

### umbracoContentXML ###

默认值是:
`~/App_Data/umbraco.config`

这个值必须设置为带波浪线(~)前缀的虚拟路径。

### umbracoContentXMLUseLocalTemp   ###
这个默认值为：
`false`

一般设置为`false`，如果当设置为`true`时，内容 XML文件（正常存储为`~/App_Data/umbraco.config`）会存储在服务器的本地临时文件夹。这对网站运行在基于文件系统SAN（非复制）的负载均衡环境中，是非常便利的。

如果你不是运行在上面的环境中，请保持它为`false`。

### umbracoContentXMLStorage (Umbraco v7.6+) ###

The default value is: `Default`

This setting replaced the `umbracoContentXMLUseLocalTemp` setting.

This setting controls where Umbraco stores the XML cache file.

The options are:

- `Default` - Umbraco cache file will be stored in `App_Data` and the `DistCache` and `PluginCache` folders will be stored in the `App_Data/TEMP` folder
- `EnvironmentTemp` - All files will be stored in the environment temporary folder
- `AspNetTemp` - All Files will be stored in the ASP.NET temporary folder

### umbracoLocalTempStorage (Umbraco v7.7.3+)

The default value is: `Default`

This setting replaced the `umbracoContentXMLStorage` setting.

This setting controls where Umbraco stores the XML cache file, the DistCache and PluginCache TEMP folders. Version 1.9.6+ of [ClientDependency Framework](https://github.com/Shazwazza/ClientDependency) also observe this setting.

The options are:

- `Default` - Umbraco cache file will be stored in `App_Data` and the `DistCache` and `PluginCache` folders will be stored in the `App_Data/TEMP` folder
- `EnvironmentTemp` - All files will be stored in the environment temporary folder
- `AspNetTemp` - All Files will be stored in the ASP.NET temporary folder

### umbracoVersionCheckPeriod

默认值是:
`7`

当这个值设置为大于0，后台会在每隔'x'天后检查新版本的 Umbraco，而这个'x'正是在这里定义的。默认情况下 Umbraco设置这个值为'7'。

### umbracoDisableElectionForSingleServer (Umbraco v7.6+)

The default value is: `false`

This is not a setting that commonly needs to be configured.

This value is primarily used on Umbraco Cloud for a small startup performance optimization. When this is true, the website instance will automatically be configured to not support load balancing and the website instance will be configured to be the 'master' server for scheduling so no [master election](https://our.umbraco.com/documentation/Getting-Started/Setup/Server-Setup/load-balancing/flexible#scheduling-and-master-election) occurs. This will save 1 database call during startup. 