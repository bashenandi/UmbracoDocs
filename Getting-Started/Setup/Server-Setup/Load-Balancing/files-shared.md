# SAN/NAS/Clustered File Server/Network Share上的文件共享

_文档是关于使用共享文件系统来设置负载均衡环境_

## 概述

配置你的服务器，使所有 IIS 实例使用共享集中式文件系统，是比较复杂的，需要一段时间才能正常完成。

使用这种方式将你的文件集中存储，你**必须**确保你的文件存储系统是 HA(Highly Available)的，这意味着它不能出现一点错误。如果你在文件服务器共享上托管你的文件，你需要使用文件共享集群(使用 [MSCS](http://en.wikipedia.org/wiki/Microsoft_Cluster_Server) 或者类似的)。Windows Server 2008支持通过[iSCSI](http://en.wikipedia.org/wiki/ISCSI)直接连接到 SAN，前提是你的 SAN 支持（也有许多其他方式可以连接到 SAN），另外你也可以通过 UNC 路径连接到 NAS，但是必须确保是 Windows/NTFS 文件系统 - IIS 不能正常使用 Linux 文件共享系统。

这里还需要做一些事情才能让它工作起来，但是一但完成之后，维护是非常简单的。对于多数站点来说，还有一些类似的设置工作，希望可以帮助你入门：

### Umbraco XML 缓存文件

一个重要的配置配置项，是当使用集中式存储时，**必须**将 umbraco.config 文件存储在各个服务器的本地中。

For **Umbraco v7.6+**

	<add key="umbracoContentXMLStorage" value="EnvironmentTemp" />

此设置会将`umbraco.config`，存储在系统环境变量的临时目录中

**或者**

	<add key="umbracoContentXMLStorage" value="AspNetTemp" />

此设置会将`umbraco.config`，存储在 ASP.NET的临时目录中

For **Umbraco Pre v7.6**

	<add key="umbracoContentXMLUseLocalTemp" value="true" /> 

此设置会将`umbraco.config`，存储在 ASP.NET的临时目录中


## Lucene/Examine 配置

你不能在服务器之间共享索引，所以当使用共享文件服务时，Examin 设置有一点技巧。

#### Examine v0.1.83+ ####

Examine v0.1.83 引入了新的 `directoryFactory`，名为`TempEnvDirectoryFactory`，你需要把它添加到`~/Config/ExamineSettings.config` 文件的所有索引器中

    directoryFactory="Examine.LuceneEngine.Directories.TempEnvDirectoryFactory,Examine"

`TempEnvDirectoryFactory` 允许 Examine 把索引文件直接存储在环境变量的临时存储目录中。

#### Pre Examine v0.1.83 ####

* 在ExamineIndex.config配置文件中, 你可以标记每台服务器的路径包含机器名, 这可以确保你在每台机器上的索引都存储在不同的位置。一个标记化的路径为：`~/App_Data/TEMP/ExamineIndexes/{machinename}/Internal/`
* 在 ExamineSettings.config配置文件中, 你可以给每个索引器和搜索器都增加这个属性`useTempStorage="Sync"`
* 这个'Sync'设置，可以将你的索引存储在本地文件系统中的 ASP.NET 临时目录。由于 Lucene 只能读取/操作本地文件，因此操作远程共享文件会出现问题。任何时候当索引更新时，这个设置会保证本地索引被创建以及索引可以被正常写入。这样同时会确保，当应用重启或者本地临时文件被清空时，索引都可以从集中存储的索引中获取并重新存储在本地。如果你在日志文件中，看到同步过程发生错误，你也可以设置这个值为'LocalOnly'使索引文件仅保留在本地。

## Windows 设置

* 创建域账户用于运行 IIS 网站。例如：MyDomain\WebsiteUser
* 给予这个域用户操作你共享文件的完全操作权限
* 在每台服务器上，将这个用户添加到 IIS 安全账户组中。Server 2003: IIS_WPG, Server 2008: IIS_IUSRS
* 必须在每台服务器上设置.NET Code Access Policy为完全信任模式，用于UsterNC共享:
** 例如: %windir%\Microsoft.NET\Framework64\v2.0.50727\caspol -m -ag 1. -url "file://\\fileserver.mydomain.local\Inetpub\MySite\*" FullTrust -name "WebsiteUser"
* 为上面的 IIS 用户，设置适当的 IIS 操作权限:
** EXAMPLE: %windir%\Microsoft.NET\Framework64\v2.0.50727\Aspnet_regiis.exe -ga ActiveDirectoryDomain\ProcessIdentity
* 重启服务器

**以上大部分内容都包含在微软文档中: [ASP.NET 3.5 主机](http://wiki.dev/GetFile.aspx?File=Wiggles-Hosting/ASPNET35_HostingDeploymentGuide.doc)**

## IIS 设置

由于所有的网站文件都集中管理，所以每台服务器的 IIS 都需要将网站指定到同样的UNC 共享中。例如：*\\\\fileserver.mydomain.local\Inetpub\MySite*

* 指向共享文件服务器: \\\\fileserver.mydomain.local\Inetpub\MySite
* 使用上面的账户"连接到……"这个共享地址
* 应用程序池运行用户均为上面设置的用户
* 给 IIS 设置匿名访问用户(IIS 7)