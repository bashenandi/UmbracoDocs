# 在 Umbraco 中使用 MVC 局部视图

_这部分会展示出如何在 Umbraco 中使用 MVC 局部视图。请注意，这篇文档和使用原生的 MVC 局部视图是相关的，而不是'[局部视图宏](../Macros/Partial-View-Macros/index.md)'_

## 概述

实际上，在 Umbraco 中使用局部视图，类似于在正常的 MVC 项目中使用局部视图。在网上有一些关于如何创建和使用 MVC 局部视图的详细文档，例如：

* [http://www.asp.net/mvc/videos/mvc-2/how-do-i/how-do-i-work-with-data-in-aspnet-mvc-partial-views](http://www.asp.net/mvc/videos/mvc-2/how-do-i/how-do-i-work-with-data-in-aspnet-mvc-partial-views)

局部视图允许你在不同的视图（模板）之间简便得重用组件。

## 视图文件位置

在 Umbraco 管道输出局部视图时，会去下面的位置查找存储文件：

	~/Views/Partials

标准的 MVC 局部视图，还可以在下面的位置正常工作：

	~/Views/Shared
	~/Views/RenderMvc

`~/Views/RenderMvc`之所以有效是因为执行绘制的控制器代码是：`Umbraco.Web.Mvc.RenderMvcController`

然而你要是使用了[Hijacking an Umbraco route](custom-controllers.md) ，并且指定了你自己的控制器来执行，你的局部视图位置还可以是：

	~/Views/{YourControllerName}

## 示例
一个快速示例是一个内容节点通过模板输出一个局部视图，用来遍历它的每一个子文档：

文档的 MVC 模板标记：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
	    Layout = null;
	}

	<html>
	<body>

		@foreach(var page in Model.Content.Children.Where(x => x.IsVisible())){
			<div>
				@Html.Partial("ChildItem", page)
			</div>
		}

	</body>
	</html>

局部视图 (位于: `~/Views/Partials/ChildItem.cshtml`)

	@model IPublishedContent

	<strong>@Model.Name</strong>

# 强类型局部视图
通常来说你会创建一个使用`@model MyModel`语句的局部视图。然而，在Umbraco 中你可能想在视图中方便的使用可用属性，好比 Umbraco helper：`@Umbraco`以及 Umbraco 上下文：`@UmbracoContext`。好消息是这是完全可以的。替换掉使用的`@model MyModel`语句，你只需要从正确的视图类继承，因此使用下面的语句替换：

	@inherits Umbraco.Web.Mvc.UmbracoViewPage<MyModel>

通过继承这个视图，您可以立即访问这些方便的属性，并使用强类型的自定义模型创建视图。

另一种情况是，你可能想要你的局部视图和正常模板一样，通过IPublishedContent的实例化，成为模型类型（`RenderModel `）的强类型。为此，你只要修改你的局部视图继承自`Umbraco.Web.Mvc.UmbracoTemplatePage`（就像你的正常视图）。当你输出你的局部视图时，你可以通过`IPublishedContent `的实例来替换`RenderModel `的新实例。例如：

	@foreach(var child in Model.Content.Children())
	{
		@Html.Partial("MyPartialName", child)
	}

局部视图依然可以继承自`Umbraco.Web.Mvc.UmbracoTemplatePage`，只是有了模型`RenderModel `，但是你也依然可以使用`IPublishedContent `的实例，并且它会自动为你创建一个新的`RenderModel `。当然你也可以创建你自己的`RenderModel `：

	@foreach(var child in Model.Content.Children())
	{
		@Html.Partial("MyPartialName",
			new global::Umbraco.Web.Models.RenderModel(child, Model.CurrentCulture))
	}

他们会得到同样的结果.

## 缓存
正常情况下你不需要缓存局部视图的输出，就像正常情况下你不需要缓存用户空间的输出，但是在有些时候他们是必要的。就想宏缓存，我们替换局部视图的输出缓存。这可以简单地通过使用 HtmlHelper 扩展方法完成：

	@Html.CachedPartial("MyPartialName", new MyModel(), 3600)

上面你的局部视图输出会缓存一小时（3600秒）。另外，你还可以为这个方法指定一些可选参数。这有完整的方法签名：

	IHtmlString CachedPartial(
		string partialViewName,
		object model,
		int cachedSeconds,
		bool cacheByPage = false,
		bool cacheByMember = false,
		ViewDataDictionary viewData = null,
		Func<object, ViewDataDictionary, string> contextualKeyBuilder = null)

因此你可以指定会员缓存和/或页面缓存，以及指定额外的视图数据给你的局部视图。**然而**，如果你的视图数据是动态的（意味着每个页面请求都会改变），缓存输出会持续返回。同样的问题会发生在如果你使用了动态模型。请注意这些：如果你在每个页面请求都使用了不同的模型或视图数据，只会缓存第一次的返回结果。如果这不是你期望的，你可以生成你自己的缓存 key，然后使用`contextualKeyBuilder `参数用于不同的缓存实例。

缓存仅在你的应用设置`debug="false"`时可用。当`debug="true"`时缓存是禁用的。同时，所有CachedPartials缓存都会在 Umbraco 发布事件时清空。