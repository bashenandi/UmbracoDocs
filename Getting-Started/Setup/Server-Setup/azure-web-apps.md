# 在Azure Web Apps上运行 Umbraco #

_本节内容描述了在Azure Web Apps上运行 Umbraco 的最佳实践_

## 什么是 Azure Web Apps ##

之前有很多别名可以称呼它，许多人人为 Azure Web Apps 就是 Azure Web Sites（*具体请参考官方解释*）。

> App Service is a fully Managed Platform for professional developers that brings a rich set of capabilities to web, mobile and integration scenarios. Quickly create and deploy mission critical web Apps that scale with your business by using Azure App Service.

[你可以在这里获取更多信息](https://azure.microsoft.com/en-us/documentation/articles/app-service-web-overview/)

Umbraco 可以运行在Azure Web Apps，但是需要做一个特殊的设置，同时你要意识到 Azure Web Apps的环境会有一定的局限性。

## 存储 ##

第一件需要知道的重要事情是Azure Web Apps使用了远程文件共享。
这意味着你的网站运行所需的本地文件，并不在提供网站服务的同一台服务器上。
在多数情况下，这不是一个问题，但是如果你有大量的 IO 操作这会成为一个问题，例如 Lucene 索引文件，频繁的操作会导致远程共享文件的延迟。

## 弹性计算 ##

如果你需要Azure Web Apps的弹性计算，你需要前往[负载均衡](load-balancing.md) 了解更多的设置，用于支持弹性计算。

## 网站迁移 ##

你要知道一个很重要的事情，Azure Web Apps可能会再任何时间点在你的网站进程之间移动网站。 这是透明的正常操作，但是如果你的任何代码或者库使用了下面的变量，可能会受到影响：

* Environment.MachineName (或者类似)
* HttpRuntime.AppDomainAppId (或者类似)

当你的网站迁移到其他进程，这个变量会发生变化。
在网站的生命周期内，你不能期待这些变量保持不变。


## 最佳实践 ##

这些最佳实践仅适用于单一环境或者非弹性计算的Azure网站。 __如果你需要Azure Web Apps的弹性计算，你需要前往[负载均衡](load-balancing.md) 了解更多的设置__，用于支持弹性计算。

* 你需要确保 `fcnMode="Single"` 存在于你的 web.config文件 `<httpRuntime>` 节点中 (对于 Umbraco 来说，这是默认的设置, 查看 [这里](http://shazwazza.com/post/all-about-aspnet-file-change-notification-fcn/) 了解更多信息)
* 你需要修改运行的网站中的Config/log4net.config文件，设置你的log4net最小级别为"WARN"(如果在调试中，这是无效的)
* 推荐的最小Azure SQL Tier 为 "S2", 如果你需要显著的提升性能，可以选择更高的 Tiers 

#### Examine v0.1.80+ ####

Examine v0.1.80 引入了一种名为`SyncTempEnvDirectoryFactory`的新 `directoryFactory`，这会为每个`~/Config/ExamineSettings.config`文件的所有索引项增加这个设置。

    directoryFactory="Examine.LuceneEngine.Directories.SyncTempEnvDirectoryFactory,Examine"

`SyncTempEnvDirectoryFactory` 允许Examine 在远程文件系统和本地临时存储目录中同步文件，再从临时存储目录中访问索引。由于 Lucene 自身原因和Azure Web Apps 的IO延迟，必须设置这个选项。

#### Pre Examine v0.1.80 ####

* 如果在`~/Config/ExamineIndex.config`文件中，有{machinename}令牌，请删除这部分内容。例如，你有路径类似于: `~/App_Data/TEMP/ExamineIndexes/{machinename}/External/` ，将其改为 `~/App_Data/TEMP/ExamineIndexes/External/` 
* 由于 Lucene 和 IO 延迟问题，你需要在`~/Config/ExamineSettings.config`文件中更新所有的Indexers 和 Searchers，增加下面的属性(访问 [here](http://issues.umbraco.org/issue/U4-7614) 了解更多): `useTempStorage="Sync"`

### Umbraco XML 缓存文件和其他临时文件 ###
对于单个实例的Azure Web App，你需要确保Umbraco XML配置文件存储在本地 (因为A Azure使用共享文件系统)。为此你需要在 web.config 中增加新的 app setting：

针对 **Umbraco v7.7.3+**

For Umbraco installations that are hosted by Azure Web Apps it is recommend that Umbraco is upgraded to the latest version if the current version is pre v7.7.3. This is so that the umbracoLocalTempStorage setting can be utilised to avoid locking issues with TEMP files during automated server migrations or slot swapping. See [U4-10503](http://issues.umbraco.org/issue/U4-10503) for more information on this.

	<add key="umbracoLocalTempStorage" value="EnvironmentTemp" />

This will set Umbraco to store `umbraco.config` and the other Umbraco TEMP files in the environment temporary folder. More info on this setting is available [here](../../../Reference/Config/webconfig/index.md#umbracolocaltempstorage-umbraco-v773)

For **Umbraco v7.6+**

	<add key="umbracoContentXMLStorage" value="EnvironmentTemp" />

这会使Umbraco将 `umbraco.config`存储在环境变量临时目录

针对 **Umbraco Pre v7.6**

	<add key="umbracoContentXMLUseLocalTemp" value="true" /> 


这会使Umbraco将 `umbraco.config`存储在 ASP.NET 临时目录