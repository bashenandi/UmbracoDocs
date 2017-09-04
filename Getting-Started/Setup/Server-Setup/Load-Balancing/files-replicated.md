# 基于文件复制的文件存储

_文档是关于使用文件复制来设置负载均衡环境_

##复制技术

在 Windows 服务器中常见的文件复制技术，是使用[DFS](http://msdn.microsoft.com/en-us/library/windows/desktop/bb540031(v=vs.85).aspx), 它包含在 Windows 服务版中。

附加的 DFS 资源:

* [Implementing DFS Replication in Windows Server 2003](http://www.windowsnetworking.com/articles_tutorials/Implementing-DFS-Replication.html)
* [Overview of DFS Replication in Windows Server 2008 R2](http://technet.microsoft.com/en-us/library/cc771058.aspx)
* [Watch an intro to installing and working with DFS](http://www.youtube.com/watch?v=DYfBoUt2RVE)

还有其他一些文件复制的替代方案，一些是免费的一些是授权的。你需要根据自行选择选择更加适合你环境的解决方案。

##非复制文件

当使用文件复制发布 Umbraco 到负载均衡环境是，有个很重要的问题是你要知道并不是所有文件都需要复制 - 否则，你将会遇到文件锁的问题。这里有一些文件和目录不能被复制：

* ~/App_Data/TEMP/*
* ~/App_Data/umbraco.config 
	* 一个替代方案，是你可以修改 web.config 文件中如下的配置，存储这个文件到 ~/App_Data/TEMP 
	
			<add key="umbracoContentXML" value="~/App_Data/TEMP/umbraco.config" />
	* 另外一个替代方案是存储 umbraco.config 文件到本地服务器的"temp"目录。实现这个，你只需将 web.config 文件中设置选项为 true。但是带来的问题是，如果你要查看这个文件，则不得不在临时文件中查找。由于文件很多，想找到并不是一直那么容易的。
			
			<add key="umbracoContentXMLUseLocalTemp" value="true" /> 
* ~/App_Data/Logs/*
	* 这是**可选的**，同时取决于你想如何配置日志（看下面） 

如果因为某些原因，你的文件复制方案，不允许你不复制某些文件或文件夹，你可以使用虚拟目录作为另一种方案来解决。*这不是推荐的配置方法，但这是一个可行的替代方案:*

* 编辑 /web.config 文件，修改 umbracoContentXML 的设置为 ~/App_Data/TEMP/umbraco.config。
* 复制每台服务器上的 /App_Data/TEMP 目录，将其拷贝到位于文件复制方案之外的唯一文件夹中。
* 在App_Data 目录中创建虚拟目录（不是虚拟应用），命名为 TEMP 。将虚拟目录指向到第二步创建的文件夹。
* 你可以在文件系统中删除 /App_Data/TEMP 目录 - 注意不是在 IIS 中，否则会删除虚拟目录。

##IIS设置

文件复制环境中的IIS 的配置相当简单。IIS 只会读取本地文件系统，就像正常的 IIS 网站一样。