# 使用 Umbraco 的服务 APIs #
_无论何时需要修改 Umbraco 存储在数据库中的实体 - 有一些服务 API 正是为此而存在。这意味着，你可以用你自定义的代码直接创建、修改和删除任何 Umbraco 的核心实体。_

## 操作 Umbraco 服务 ##
要使用服务 API - 你必须先获取他们。这可以通过` ApplicationContext `，来操作 Umbraco 应用程序提供的一切相关信息。


### 通过Controller ###
如果在你自己的控制器类中操作 Umbraco 服务 - 通过继承某个 Umbraco 的基础控制器，使用`Services `来获取所有的服务：

	public class EventController : Umbraco.Web.Mvc.SurfaceController{
		public Action PerformAction(){
			var content = Services.ContentService.GetById(1234);
		}
	}
	
### 通过ApplicationEventHandler ###
如果我们的案例要订阅ApplicationStarted事件 - 我们可以通过上下文获取操作 - 提供的`.Services`包含了所有可用的服务：

	public class EventHandler : Umbraco.Core.ApplicationEventHandler{
		protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext){
			var c = applicationContext.Services.ContentService.GetById(1234);
			applicationContext.Services.ContentService.Delete(c);
		}
	}

## 可用服务
这里是完整的 api，涵盖了所有Umbraco 的核心实体：

- [ConsentService](../../../Reference/Management/Services/ConsentService.md)
- [ContentService](../../../Reference/Management/Services/ContentService.md)
- [ApplicationTreeService](../../../Reference/Management/Services/TreeService.md)
- [DataTypeService](../../../Reference/Management/Services/DataTypeService.md)
- EntityService
- [FileService](../../../Reference/Management/Services/FileService.md)
- [LocalizationService](../../../Reference/Management/Services/LocalizationService.md)
- MacroService
- [MediaService](../../../Reference/Management/Services/MediaService.md)
- [MemberService](../../../Reference/Management/Services/MemberService.md)
- [MemberTypeService](../../../Reference/Management/Services/MemberTypeService.md)
- [MemberGroupService](../../../Reference/Management/Services/MemberGroupService.md)
- [ContentTypeService](../../../Reference/Management/Services/ContentTypeService.md)


### 更多信息
- [Umbraco 服务 API 参考](../../../Reference/Management/Services/)
- [Umbraco 事件参考](../../../Reference/Events/)
- [路由和控制器](../../../Reference/Routing/)

### Umbraco TV
- [Chapter: Content API](https://umbraco.tv/videos/umbraco-v7/developer/fundamentals/content-api/)
- [Chapter: Media API](https://umbraco.tv/videos/umbraco-v7/developer/fundamentals/media-api/)
- [Chapter: Member API](https://umbraco.tv/videos/umbraco-v7/developer/fundamentals/member-api/)