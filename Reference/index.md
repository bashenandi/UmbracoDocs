#开发人员参考

_开发人员参考主要包含不同的Umbraco核心API参考。在许多时候，参考会包含一些简单的代码片段示例。要对不同的 APIs进行更加深入的学习，在文档的"使用 umbraco"和"扩展 umbraco "部分可以学到更多。_

##[常见陷阱和反模式](Common-Pitfalls/index.md)
这部分格外重要！它描述了开发人员容易陷入的许多的常见陷阱。确保你阅读了这个部分 - 它可能会挽救你的网站！

##API 文档

####[C# API 文档](https://our.umbraco.org/apidocs/csharp/)

__注意: 打开文档浏览你会看到与当前文档的不同，我们有计划将它们整合到一起。__

C# API references for the Umbraco Core and Web libraries

####[后台 UI API 文档](https://our.umbraco.org/apidocs/ui/)

__注意: 打开文档浏览你会看到与当前文档的不同，我们有计划将它们整合到一起。__

创建 Umbraco 后台组件所需的Angular, JS, CSS & Less UI API 参考

##[安全](Security/index.md)
Umbraco 安全信息，它包含许许多多安全选项和配置项，用于任何和授权如何在 Umbraco 中工作

##[配置](Config/index.md)
关于所有 Umbraco 的配置文件和选项的信息

##[控制反转和依赖注入](using-ioc.md)
关于配置 IoC 和 DI 如何在 Umbraco 中运行的信息

##[模板](Templating/index.md)
Umbraco 模板相关：视图、局部视图、子方法，Razor 语法，宏和使用 JS/CSS

##[查询和模型](Querying/index.md)
用于在Umbraco中显示内容的数据模型，和许多查询它们的方式。

##[路由和控制器](Routing/index.md)
Umbraco 内部路由是如何工作的，内容是如何映射到 URLs 和如何从内容生成 URLs。
本部分内容还描述了可以在 Umbraco 中使用的控制器种类，它们是如何工作的以及他们如何获取路由。

##[搜索](Searching/index.md)
关于如何在你的 Umbraco 中使用 Examine实现搜索的功能，这是基于 Lucene 的搜索引擎。

##[事件](Events/index.md)
事件模型包含了所有在系统中触发自定义代码或自动化的主要信息。

##[Rest APIs](Routing/WebApi/index.md)
如何使用[Web API](http://www.asp.net/web-api)通过 Umbraco 创建 REST 服务。

##[管理 APIs](Management-v6/index.md)
着重于创建、更新和删除的 API。

##[插件](Plugins/index.md)
术语"插件"是指可以在 Umbraco 中找到的任何用于扩展、增强 Umbraco 的应用程序。

##[缓存](Cache/index.md)
描述如何Umbraco中如何缓存自定义数据结构。如果你要创建 Umbraco 包，可能会包含一些自定义数据源和你想要缓存的某些数据，非常有助于我们理解 Umbraco 中缓存是如何工作的，以及理解在负载均衡环境中的影响。