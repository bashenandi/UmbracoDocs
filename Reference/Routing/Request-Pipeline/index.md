# Umbraco 中的路由 #

_本节描述了 Umbraco 中的请求管道是什么，Umbraco 如何匹配一个文档到给定的请求，以及如何生成一个文档的 URL。_

## 请求管道 ##

### 什么是管道 ###

请求管道是为节点建立URL、将请求解析到指定节点并确保返回正确内容的过程。

![什么是管道](images/what-is-the-pipeline.png)

有关详细说明，请参阅 [Stephan's slide deck for his presentation on the Umbraco Pipeline](https://our.umbraco.com/documentation/Reference/Routing/Request-Pipeline/document/TheUmbracoRequestPipeline.pdf).

### 出站和入站
管道是双向工作的：**[inbound](inbound-pipeline.md)** 和 **[outbound](outbound-pipeline.md)**。

**[Outbound](outbound-pipeline.md)** 是为一个请求的节点构建 URL 的过程。**[Inbound](inbound-pipeline.md)** 是由Web 服务器接收并由 Umbraco处理的每个请求。

### 定制化管道 ###
本节描述了你可以用来修改 Umbraco 请求管道的组件：**[IContentFinder](IContentFinder.md)** & `IUrlProvider`
