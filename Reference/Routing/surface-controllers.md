# Surface Controllers #

_SurfaceController是一个MVC控制器，它与Umbracopage的前端渲染交互。它们可以用于呈现MVC子操作以及处理表单数据提交。SurfaceControllers是自动路由的，这意味着您不必为这些控制器添加/创建自己的路由就可以工作。_

## SurfaceController是什么? ##

它是一个标准的ASP.NET MVC控制器：

* 是自动路由的，意味着您不必配置任何自定义路由使其工作
* 用于和 Umbraco 的前端（不是后台）交互

由于所有的SurfaceController都继承自`Umbraco.Web.Mvc.SurfaceController`，因此该类可以立即支持许多基本SurfaceController类上可用的辅助方法和属性，包括`UmbracoHelper` and `UmbracoContext`。

因此，所有Surface控制器都具有本地的Umbraco支持：

* 在 HTTP 提交期间与 Umbraco 路由交互(i.e. `return CurrentUmbracoPage();` )
* 在 Umbraco 中渲染表单 (i.e. `@Html.BeginUmbracoForm<MyController>(...)` )
* 渲染 ASP.NET MVC 子方法 

## 创建一个SurfaceController ##

SurfaceControllers是一个插件，意味着当 Umbraco 应用启动时会被发现。有两种类型的SurfaceController：**本地定义**和**基于插件**。主要区别在于基于插件的控制器通过 MVC 的 Area进行路由，MVC Area 在控制器中定义（见下文）。因为基于插件的控制器是通过MVC区域路由的，这意味着视图可以存储在指定包的自定义目录中，而不会干扰本地开发人员的MVC文件。

### 本地声明的控制器 ###

本地声明的SurfaceController不是在Umbraco包中运行的。它是由创建网站的开发人员所创建。如果您计划在Umbraco包中承载SurfaceController，则需要创建基于插件的SurfaceController（请参见下一标题）。

创建一个本地的SurfaceController: 

* 创建一个控制器，继承自 `Umbraco.Web.Mvc.SurfaceController`
* 控制器必须是 public 类.
* 控制器名的后缀必须是`Controller`
* 控制器必须位于一个命名空间内

示例:

```csharp
namespace name.Core.Controllers
{
    public class MyController : Umbraco.Web.Mvc.SurfaceController
    {
        public ActionResult Index() 
        {
            return Content("hello world");
        }
    }
}
```

#### 本地声明的控制器的路由 ####

所有本地声明的控制器都被路由到:

    /umbraco/surface/{controllername}/{action}/{id}

它们不会被 MVC Area 路由，所以任何视图文件都必须位于下面的目录中：

* `~/Views/{controllername}/`
* `~/Views/Shared/`
* `~/Views/`

:::tip
如果当你访问你的Surface控制器，而获取到404错误时，可能是你忘了给它添加一个命名空间！
:::

## 基于插件的控制器 ##

如果你要在一个包中运行SurfaceController，你一定要创建一个基于插件的SurfaceController。创建基于插件的控制器和本地控制器仅有的区别在于，你需要添加一个属性到你的类，该属性定义了您希望控制器通过的 MVC Area。这是示例：

```chsarp
namespace name.Core.Controllers
{
	[PluginController("SuperAwesomeAnalytics")]
	public class MyController : Umbraco.Web.Mvc.SurfaceController
	{
		public ActionResult Index() 
		{
			return Content("hello world");
		}
	}
}
```

在上面，我门指定了希望我的MyController属于名为“SuperAwesomeAnalytics”的MVC区域。也许这是显而易见的，但是如果您正在创建一个包含许多SurfaceController的包，那么您应该最明确地确保所有控制器都通过相同的MVC区域进行路由。

#### 基于插件控制器的路由 ####

所有基于插件的控制器都被路由到：

    /umbraco/{areaname}/{controllername}/{action}/{id}

如果它们通过 MVC Area 路由，你的视图可以位于下面的目录中：

* `~/App_Plugins/{areaname}/Views/{controllername}/`
* `~/App_Plugins/{areaname}/Views/Shared/`
