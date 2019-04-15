# 配置文件 #

_本节将解释umbraco中的许多配置选项以及它们的作用。_

## [web.config](webconfig/index.md)
此文件可以在以下路径找到：/web.config

## [404handlers.config](404handlers/index.md)
旧版NotFoundHandlers配置文件

此文件可以在以下路径找到： /config/404handlers.config

## [applications.config](applications/index.md)
在Umbraco后台设置分区的配置选项。

此文件可以在以下路径找到: /config/applications.config

## [dashboards.config](dashboard/index.md)
用来在后台中控制哪个仪表板显示在哪个板块中，以及谁有权限看到它们的配置项。

此文件可以在以下路径找到: /config/Dashboard.config


## [EmbeddedMedia.config](EmbeddedMedia/index.md)
这个配置文件列出了在你的 Umbraco 中可以使用的嵌入式媒体提供者的配置内容。

此文件可以在以下路径找到: /config/EmbeddedMedia.config

## [ExamineIndex.config](ExamineIndex/index.md)
'ExamineIndex.config'文件包含了配置检索索引用于存储索引内容到 Umbraco 安装中的配置。

此文件可以在以下路径找到：/config/ExamineIndex.config

## [ExamineSettings.config](ExamineIndex/index.md)
'ExamineSettings.config'文件包含了配置检索索引用于存储索引内容到 Umbraco 安装中的配置。

此文件可以在以下路径找到: /config/ExamineSettings.config

## [fileSystemProviders.config](fileSystemProviders/index.md)
'fileSystemProviders.config' 文件包含umbraco用于与文件系统交互的文件系统提供程序的配置。

此文件可以在以下路径找到: /config/FileSystemProviders.config


## [BaseRestExtensions.config](BaseRestExtensions/index.md)

'BaseRestExtension.config'包含在类库中公开方法时/base系统所需的数据。

此文件可以在以下路径找到: /config/BaseRestExtensions.config

## [tinyMceConfig.config](tinyMceConfig/index.md)
'tinyMceConfig.config'包含了在 Umbraco 后台中使用的[TinyMce](https://www.tinymce.com/)编辑器的配置项。

此文件可以在以下路径找到: /config/tinyMceConfig.config

## [trees.config](trees/index.md)
'trees.config'包含了每个板块中的树的配置。

此文件可以在以下路径找到: /config/trees.config

## [umbracoSettings.config](umbracoSettings/index.md)

包含许多 Umbraco 选项。一般来说默认配置适用于绝大多数安装实例；但是，在某些情况下，其中一些选项可能需要根据每个安装进行调整。

此文件可以在以下路径找到: /config/umbracoSettings.config

## clientdependency.config ##

ClientDependency 配置选项可以在这里找到 [ClientDependency 网站](https://github.com/Shandem/ClientDependency/wiki/Configuration).

## log4net.config ##

log4net配置选项可以在这里找到 [log4net 网站](http://logging.apache.org/log4net/release/manual/configuration.html).

## UrlRewriting.config
目前它已经过时了，在 [IIS](https://docs.microsoft.com/en-us/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) 中有更好的重写方式。但是，对于旧的项目，你也可以[下载 PDF 格式](https://github.com/aspnetde/UrlRewritingNet/blob/master/docs/UrlRewritingNet20_English.pdf) 的UrlRewriting 文档。