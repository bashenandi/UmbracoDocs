#Web.config

_这部分定义了可以在 web.config中找到的 appsetting 参数_

##明确的设置

_这些web.config 文件中 appSetting 部分的设置，必须要一个明确的值_

###umbracoConfigurationStatus
保持这个版本号为当前安装的 Umbraco 版本。这个版本号会在运行安装或者升级安装时自动更改。不建议手工更改这个值，升级安装需要在升级的网站时执行完一些动作后更改这个值。当这个版本号和Umbraco DLL的版本号一样时，升级安装器是不会运行的。

###umbracoReservedUrls
逗号分隔的 Umbraco 需要单独留下的文件。IIS 将保留这些文件，Umbraco 的请求管道不会触发这些文件。

    <add key="umbracoReservedUrls" value="~/config/splashes/booting.aspx,~/install/default.aspx,~/config/splashes/noNodes.aspx,~/VSEnterpriseHelper.axd" />

###umbracoReservedPaths
逗号分隔的 Umbraco 需要单独留下的文件夹。如果你有文件夹包含一些自定义文件，将其添加到这个设置中，Umbraco 会一直保留它们。

    <add key="umbracoReservedPaths" value="~/umbraco,~/install/" />

###umbracoPath

URL 指向 Umbraco 管理文件夹。如果你重命名了`umbraco`文件夹，你也需要更新这个设置。

    <add key="umbracoPath" value="~/umbraco" />

###umbracoHideTopLevelNodeFromPath
如果你运行了多个网站，你不会想最高层级的节点显示在 url 中。可用的选项是`true`和`false`。

    <add key="umbracoHideTopLevelNodeFromPath" value="true" />

###umbracoUseDirectoryUrls

Strips `.aspx` from URLs on the frontend when set to `true`.  
This setting is only important to older IIS configurations where extension-less URLs weren't supported very well.

###umbracoTimeOutInMinutes
配置为当Umbraco 用户没有任何请求后的多少分钟，需要用户重新登录。任何后台请求都会重置这个时钟。默认设置是20分钟。

    <add key="umbracoTimeOutInMinutes" value="20" />

###umbracoDefaultUILanguage
如果没有为用户显式的指定语言时，登录后台默认显示的语言。默认是English(en)。

###umbracoUseSSL
当设置为`true`时，要求所有后台的请求都使用 HTTPS 协议来替代 HTTP。

### umbracoCssDirectory
通过添加这个值你可以指定一个新的文件夹用于存储你的 css 文件，并在可以在 Umbraco 中编辑它们。默认文件夹是：`~/css`。

### umbracoMediaPath
通过添加这个值你可以指定一个新的文件夹用于存储你的媒体文件。默认文件夹是：`~/media`。

### umbracoScriptsPath
通过添加这个值你可以指定一个新的文件夹用于存储你的 css 文件，并在可以在 Umbraco 中编辑它们。默认文件夹是：`~/scripts `。

##选择设置

_这些设置有默认值，但是能够通过在 web.config 中创建新的 appSetting 并设置它们的值来重写_

###umbracoContentXML

默认值是:
`~/App_Data/umbraco.config`

这个值必须设置为带波浪线(~)前缀的虚拟路径。


###umbracoContentXMLUseLocalTemp
这个默认值为：
`false`

一般设置为`false`，但是当设置为`true`时，内容 XML文件（正常存储为`~/App_Data/umbraco.config`）会存储在服务器的本地临时文件夹。这对网站运行在基于文件系统SAN（非复制）的负载均衡环境中，是非常便利的。

如果你不是运行在上面的环境中，请保持它为`false`。

###umbracoVersionCheckPeriod

默认值是:
`7`

当这个值设置为大于0，后台会在每隔'x'天后检查新版本的 Umbraco，而这个'x'正是在这里定义的。默认情况下 Umbraco设置这个值为'7'。
