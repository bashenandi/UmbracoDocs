# 灵活的负载均衡 #

_关于将 Umbraco 发布到灵活的负载均衡环境的方案和其他一些细节的信息。_

**_低于7.3.0版本的 Umbraco必须使用传统的负载均衡方案。_**

**在开始之前请确保你已经[预览](index.md)了负载均衡的基本概念!**

## 设计 ##
这些说明假设具备如下条件:

* 所有 Web servers 均可与 Umbraco 数据存储的数据库通信
* 你运行的是 Umbraco 7.3.0 及以上版本
* _**你应该指定一台单独的服务器用作后台服务器，用于编辑内容。**_ 如果后台管理在负载均衡环境中，Umbraco 无法正常工作

你可以选择使用下面的三种方案，使负载均衡服务器更加高效:

1. 使用基于云平台的弹性计算，类似于[Microsoft's Azure Web Apps](https://azure.microsoft.com/en-us/services/app-service/web/)
2. 每台服务主机复制网站文件，由文件复制服务来确保每台服务器上文件的更新
3. 负载均衡网站位于集中式的文件共享，例如(SAN/NAS/Custered File Server/Network Share)

对于#2和#3来说，你需要在你的负载均衡环境中前置一台负载均衡服务器！

## 灵活负载均衡是如何工作的 ##

为了了解主机上的网站是如何工作的，最好先了解 Umbraco 灵活负载均衡是如何工作的。

下图展示了在整个浏览环境中，数据是如何传输和通信的：:

![Umbraco 灵活负载均衡图标](images/flexible-load-balancing.png)


流程如下：

* 管理员或者编辑者，在主服务器上进行创建、更新、删除数据/内容
* 这些行为转换为称为『指令』的数据结构中，再存储在数据库队列中
* 每台前端服务器检查是否有未处理过的未完成指令
* 当前端服务器检测到待操作的指令，会下载并处理它们，再将其更新到缓存中，缓存文件及索引文件位于该服务器文件系统中
* 在内容更新与前端服务器刷新之间，有大约5秒的延迟，这是预设的正常行为。

## 调度与活动服务器 ##

虽然已经指定了一台用于管理的主服务器，但默认情况下并不是显式地设置为“调度服务器”。在 Umbraco 中作为一台单独的调度服务器，需要完成一下三件事：

* 持续活动的服务器 - 确保计划的触发
* 计划任务 - 启动任何已配置的任务
* 计划发布 - 启动所有计划发布的文档

灵活负载均衡会自动选出一台『调度服务器』来执行上面的服务。这意味着所有服务器都不得不为任何一项指定URL，包含：它自己、主服务器、负载均衡内网服务器或公网地址。

例如，在下面的图表中，从属节点**f02.mysite.local**被指定为『调度服务器』。为了完成调度的工作，它需要发送请求给它自己、主服务器、负载均衡内网地址或者公网地址。『调度服务器』使用的地址被称之为 "umbracoApplicationUrl"。

![灵活负载均衡图标](images/flexible-load-balancing-scheduler.png)

默认情况下，Umbraco 会把"umbracoApplicationUrl" 设置为应用进程启动后第一次请求的地址。他假定该地址为服务器可以解析的 DNS 地址。

例如，如果一个公网请求到达『www.mysite.com』指定的负载均衡地址，负载均衡会发送请求到原始地址的『www.mysite.com』的服务器，所以这里默认"umbracoApplicationUrl"就是"www.mysite.com"。然后，负载均衡也可能会将该请求路由至内网某个不同的服务器上，例如"f02.mysite.local"，此时，就意味着"umbracoApplicationUrl"的地址是"f02.mysite.local"。在任何情况下，被指定的『调度服务器』都需要能够解析这个地址。

在多数情况下这是没问题的，但是有时还不能达成目标，这里还有一些选择可供使用：

* __推荐__: [设置你的前端服务器（非管理服务器）到指定的从服务器](flexible-advanced.md#explicit-master-scheduling-server) ，这意味着他们永远不会被用作主调度服务器
* 在[/Config/umbracoSettings.config文件的Web.Routing节点](../../../../Reference/Config/umbracoSettings/index.md)中设置`umbracoApplicationUrl`
* 或者在[应用启动时处理`ApplicationStarting`](../../../../Reference/Events/Application-Startup.md)，你可以指定一个委托，返回设置的指定节点 [`ApplicationUrlHelper.ApplicationUrlProvider`](https://github.com/umbraco/Umbraco-CMS/blob/75c2b07ad3a093b5b65b6ebd45697687c062f62a/src/Umbraco.Core/Sync/ApplicationUrlHelper.cs#L21)

## Umbraco 配置文件 ##

这里不必更改任何 _Umbraco_ 配置文件。但是为了使灵活负载均衡工作，**一定不能开启在umbracoSettings.config文件中的 _distributeCall_ 标签**，那纯粹是为了传统负载均衡所用。 

这里需要更改一些索引/日志的的配置文件(具体请见下方和[概述](index.md))

## 选择 #1 : 基于云的弹性伸缩计算 ##

基于云应用的环境，将 Umbraco 部署至灵活负载均衡，我们主要集中在对 Azure Web Apps 的介绍。这些文档主要针对于 Azure Web Apps，但是类似的设置，也可实现其他服务平台的支持。

在开始之前，确认你已经阅读了[概述](index.md)  - 你需要确保 asp.net 和日志已经可以正常工作。

### Azure 需求 ###

* 你需要配置两台Azure Web Apps - 一台用于后端管理环境，另外一台用于前端环境
* 你需要一台SQL server，对于两台 web apps 来说它是共用的

### Lucene/Examine 配置 ###

#### Examine v0.1.80+ ####

Examine v0.1.80 引入了新的 `directoryFactory` ，称之为 `SyncTempEnvDirectoryFactory`，应该把它添加到所有的索引器中

    directoryFactory="Examine.LuceneEngine.Directories.SyncTempEnvDirectoryFactory,Examine"

`SyncTempEnvDirectoryFactory` 可以在远程和本地环境的临时文件存储目录之间同步索引，索引直接操作临时文件存储目录。这个设置之所以被需要，是因为 Lucene 从远程读取文件共享会出现问题，所以需要在本地读取/访问文件。任何时候当索引更新时，这个设置会保证本地索引被创建以及索引可以被正常写入。这样同时会确保，当应用重启或者本地临时文件被清空时，索引都可以从集中存储的索引中获取并重新存储在本地。

#### Pre Examine v0.1.80 ####

* 在 ExamineSettings.config 文件中，给你的所有索引器和搜索器都增加下面的属性： `useTempStorage="Sync" tempStorageDirectory="UmbracoExamine.LocalStorage.AzureLocalStorageDirectory, UmbracoExamine"`
* 这个 'Sync' 设置会存储你的索引在本地文件系统中，而不是远程的 Azure Web App系统中。Lucene 从远程读取文件共享会出现问题，所以需要在本地读取/访问文件。任何时候当索引更新时，这个设置会保证本地索引被创建以及索引可以被正常写入。这样同时会确保，当应用重启或者本地临时文件被清空时，索引都可以从集中存储的索引中获取并重新存储在本地。如果你在日志文件中，看到同步过程发生错误，你也可以设置这个值为'LocalOnly'使索引文件仅保留在本地，但这并不意味着，当网站在 Azure 工作区之间切换时，这些索引会自动重建。

### 如果你计划使用弹自动缩放 ###

**重要!** 你的Examine路径设置必须更新！Azure Web Apps 使用共享文件系统，这意味着当你增加了超过一个的前端工作区时，你的 Lecene 索引文件也会在超过一个进程中被共享。而这是无法工作的！

#### Examine v0.1.83+ ####

Examine v0.1.83 引入了新的 `directoryFactory` ，名为 `TempEnvDirectoryFactory` ，你需要在 `~/Config/ExamineSettings.config` 文件中给每个索引器都添加该属性

    directoryFactory="Examine.LuceneEngine.Directories.TempEnvDirectoryFactory,Examine"

`TempEnvDirectoryFactory`允许 Examine 索引直接存储到系统环境的临时存储目录中。

#### Pre Examine v0.1.83 ####

在 ExamineIndex.config 文件中，你需要给每个索引器标记包含机器名的路径，这会确保你的每台机器都将索引存储在不同的位置。一个典型的标记了机器名的路径为：`~/App_Data/TEMP/ExamineIndexes/{machinename}/Internal/`。但是有两个原因会导致一些缺陷：


* Azure web apps 在不同的工作区间迁移你的网站，并不会发出警告，此时{machinename}会发生改变，而你的索引也会重建
* 当你增加了工作区的数量时，新的工作区也会重建他自己的索引

我们正在努力通过添加一个blob 类型的存储，可以将索引存储在其中来来减轻这些问题，以便当新的工作区开始工作时时，他们可以在本地同步已有的索引（但是这些目前还没有实现）。

### Umbraco XML 缓存文件及其他临时文件 ###

对于Azure Web App的前端实例来说，你需要确保 Umbraco XML 配置文件是保存在本地服务器的（由于Azure Web apps 使用的是共享存储文件系统）。实现这个你需要在 web.config 中增加一个新的 app 设置：

For **Umbraco v7.7.3+**

	<add key="umbracoLocalTempStorage" value="EnvironmentTemp" />

This will set Umbraco to store `umbraco.config` and the other Umbraco TEMP files in the environment temporary folder. More info on this setting is available [here](../../../../Reference/Config/webconfig/index.md#umbracolocaltempstorage-umbraco-v773)

For **Umbraco v7.6+**

	<add key="umbracoContentXMLStorage" value="EnvironmentTemp" />

这会让 Umbraco 将 `umbraco.config` 存储在系统环境的临时文件目录

For **Umbraco Pre v7.6**

	<add key="umbracoContentXMLUseLocalTemp" value="true" /> 

这会让 Umbraco 将 `umbraco.config` 存储在ASP.NET的临时文件目录

### 步骤 ###

1. 创建 Azure SQL 数据库
2. 在你的管理环境中安装 Umbraco，并确保其使用的是你的 Azure SQL 数据库
3. 在你的前端环境中安装 Umbraco，并确保其使用的是你的 Azure SQL 数据库
4. 测试: 在管理环境中更新一些数据，确保其工作正常，然后验证这些更新是否出现在前端环境中

### 弹性运算 ###

**不要改变你管理环境的弹性运算** 这不被支持，并且会出现一些问题。

Azure Web Apps支持手工或自动缩放，并且支持 Umbraco 的灵活负载均衡机制的操作。

### 部署注意事项 ###

由于您有2个Web应用程序，所以当您部署时，您需要同时部

**重要注意事项:** 这也意味着你不应该在生产环境中直接编辑模板或者视图，因为主环境和前端环境，并不共享一个文件系统。

## 选择 #2 : 文件复制存储

如果你使用的不是基于云平台的设置，那么*这里是推荐的设置*。

在开始之前，确认你已经阅读了[概述](index.md)  - 你需要确保 asp.net 和日志已经可以正常工作。

[使用选项 #2 的有关具体细节，请参见此处](files-replicated.md)

### 缩放 ###

由于需要添加服务器/站点，因此弹性缩放依旧需要你进行一些简单的设置，但是对于灵活的负载均衡，你不需要在 Umbraco 中进行任何设置，你只需要设置Umbraco站点的数据库连接为同一个数据库，并且在负载均衡中加入该站点网站。

## 选择 #3 : SAN/NAS/Clustered File Server/Network Share之类的共享存储 ##

配置你的服务器使用集中式文件系统，并且在所有 IIS 实例中共享，是比较复杂的，你可能需要一些时间才能使其正常运行。

[使用选项 #3 的有关具体细节，请参见此处](files-shared.md)

### 缩放 ###

由于需要添加服务器/站点，因此弹性缩放依旧需要你进行一些简单的设置，但是对于灵活的负载均衡，你不需要在 Umbraco 中进行任何设置，你只需要设置Umbraco站点的数据库连接为同一个数据库，并且在负载均衡中加入该站点网站。

## 高级技术 ##

当你熟悉了灵活负载均衡，可能会对其他一些事情感兴趣[高级技术](flexible-advanced.md).
