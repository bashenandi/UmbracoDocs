# 计划发布 #

Umbraco 中的每个文档都可以根据预先定义的日期和时间有计划的发布和取消发布。为此，前往"Properties"标签找到"Publish at" 和 "Unpublish at"字段。在这你可以选择精确的日期和时间，来发布或者取消发布当前内容条目。

![Scheduled publishing](images/Scheduled-publishing.png)

## 时区 ##
由于你所在的位置和你的服务器可能会在不同的时区。从 Umbraco7.5开始，你可以在你本地时区中选择日期和时间，Umbraco 会确保按照选择的时间发布条目。所以如果你选择了下午12点，那么条目就会在你所在时区的下午12点发布。服务器可能会是下午8点，而这在你选择日期和时间时就已决定了。

![计划发布时区不同](images/Publish-Timezone-Difference.jpg)
如果你和你的服务器在一个时区内，这些信息不会现在在日期选择下方。

**注意:** 低于7.5版本的 Umbraco，你选择的时间是服务器时间，这些旧版本的 Umbraco 不会检测你的本地时区。

## 权限 ##

如果你有发布所选条目的权限，你可以选择日期和时间，而如果你的用户是"Writer"角色，你不能在这里选择时间。

##配置

在某些场景，你需要调整你的配置，来确保计划发布/取消发布可以正常工作。计划任务工作会通过服务器向它自己发送 HTTP(S)请求。

如果你在负载均衡环境中，要特别注意必须确保这些配置是正确的，[在这里浏览文档](../../Setup/Server-Setup/Load-Balancing/flexible.md#scheduling-and-master-election)。

如果你没有负载均衡的环境，Umbraco 会根据下面的顺序决定发送计划任务 HTTP(S)请求到哪个 URL 地址：

* umbracoSettings:settings/web.routing/@umbracoApplicationUrl 如果存在 _(查看 [这里](../../../Reference/Config/umbracoSettings/index.md#web-routing) 的详细信息)_
* 否则，如果 umbracoSettings:settings/scheduledTasks/@baseUrl 存在 _(已过时)_
* 否则，如果 umbracoSettings:distributedCall/servers 这里我们配置了服务器 _(已过时, 查看负载均衡文档)_
* 否则， 它基于网站中接收并使用此请求的第一个请求作为基本请求。 _(默认)_

如果使用了`umbracoApplicationUrl`，还会指定架构（）
If the `umbracoApplicationUrl` is used, the value also specfies the scheme (http 或者 https), 当时如果使用了任何其他选项，比如如果设置了  `umbracoUseSSL`为`true`，那么计划发布请求会一直使用 HTTPS。

## 故障排除 ##
如果你的计划发布/取消发布不能正常工作，而且看起来更像是服务器与计划任务终端无法通信。你可以通过几个原因来判断： 

* URL 重写阻碍了请求到达终端
* DNS 配置错误不允许服务器与第一次请求的基础 URL 通信 - 您的服务器站点位于防火墙/ NAT /负载均衡器之后，可能会直接影响到它
* SSL 和/或 umbracoUseSSL 配置错误，不允许服务器与计划发布终端在正确的协议下通讯

判断这些问题最好的方法是临时更改你的 log4net 配置，用 DEBUG 替换 INFO。它会把所有信息都按序显示给你，从而可以看到调度发布终端是否可以到达。 

在某些情况下，可以通过简单的设置来确保服务器与其自己通过正确的URL 链接通信[umbracoSettings:settings/web.routing/@umbracoApplicationUrl](../../../Reference/Config/umbracoSettings/index.md#web-routing)。