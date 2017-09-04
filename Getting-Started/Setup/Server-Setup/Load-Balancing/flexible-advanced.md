# 灵活负载均衡的高级技术

_这里讲述了一些进阶技术，你可以通过灵活负载均衡技术实现_

## 明确主调度服务器

建议配置显式的主调度服务器，这会减少[主调度服务器选取](flexible.md#scheduling-and-master-election)的复杂性。

这是可配置的，为了做到这一点，您需要为你的前端服务器和管理服务器编写单独的应用程序启动处理程序……可以在自己的代码中设置此配置选项。

第一件事是为你的前端和主服务器创建关联的class，代码如下：

	public class MasterServerRegistrar : IServerRegistrar2
	{
		public IEnumerable<IServerAddress> Registrations
		{
			get { return Enumerable.Empty<IServerAddress>(); }
		}
		public ServerRole GetCurrentServerRole()
		{
			return ServerRole.Master;
		}
		public string GetCurrentServerUmbracoApplicationUrl()
		{
			//NOTE: If you want to explicitly define the URL that your application is running on,
			// this wil be used for the server to communicate with itself, you can return the 
			// custom path here and it needs to be in this format:
			// http://www.mysite.com/umbraco

			return null;
		}
	}

	public class FrontEndReadOnlyServerRegistrar : IServerRegistrar2
	{
		public IEnumerable<IServerAddress> Registrations
		{
			get { return Enumerable.Empty<IServerAddress>(); }
		}        
		public ServerRole GetCurrentServerRole()
		{
			return ServerRole.Slave;
		}        
		public string GetCurrentServerUmbracoApplicationUrl()
		{
			return null;
		}
	}

然后你需要在应用启动时，替换注册默认的 `DatabaseServerRegistrar`。
你需要创建一个[ApplicationEventHandler](/Documentation/Reference/Events/Application-Startup)并且复写`ApplicationStarting`方法。在这里你可以替换注册对象：

	//这会在你的主服务器执行
	ServerRegistrarResolver.Current.SetServerRegistrar(new MasterServerRegistrar());

	//这会在你的从属服务器执行
	ServerRegistrarResolver.Current.SetServerRegistrar(new FrontEndReadOnlyServerRegistrar());

现在你的前端服务器会使用你自定义的`FrontEndReadOnlyServerRegistrar`类，他们会被一直认定是『从』服务器，而不会尝试任何主服务器选择以及任务调度工作。
同时你的主服务器会使用你定义的`MasterServerRegistrar`类，他们会被一直认定为『主』服务器，而且永远执行所有的任务调度。


## 前端服务器 - 数据库只读访问

_这段内容仅适用于 Umbraco 数据表_

在一些案例结构中，管理员不希望前端服务器有写入/操作数据库的权限。默认情况下，前端服务器对下面的表要有写入及完全操作权限：

* umbracoServer
* umbracoNode

这是因为默认情况下，每台服务器都会通知数据库它们是活动的，对任务调度来说这更加重要。只有一台服务器可以用于执行任务调度，这些表用于主服务选举过程，而不需要其他配置。因此在具体例子中，当前端服务器成为主任务调度服务器时，**它会请求所有 Umbraco 数据表的写权限**。

为了能够为前端服务器设置数据库只读访问，你需要实现上面[Explicit master scheduling server](#explicit-master-scheduling-server)部分提到的配置方法。

现在你的前端服务器会使用你自定义的`FrontEndReadOnlyServerRegistrar`类，他们会被一直认定是『从』服务器，而不会尝试任何主服务器选择以及任务调度工作，并且由于你不再使用默认的`DatabaseServerRegistrar`，他们也不会再尝试请求umbracoServer表。
