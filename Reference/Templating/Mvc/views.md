# 在 Umbraco 中使用 MVC 视图

_在 Umbraco 中使用 MVC 视图和 Razor 语法_

## 视图中可用的属性

所有的 Umbraco 的视图都继承自`Umbraco.Web.Mvc.UmbracoTemplatePage`，它在 Razor 中暴露了许多可用的属性：

* __@Umbraco__ (对应类型`Umbraco.Web.UmbracoHelper`) -> 包含了许多有用的方法，从输入宏和字段到基于 Id 来检索内容，还有其他许多有用的方法。[查看UmbracoHelper文档](../../Querying/UmbracoHelper/index.md)
* __@Html__ (对应类型 `HtmlHelper`) -> 如同你知道并喜欢的 Microsoft 提供的 HtmlHelper 方法，我们添加了一些更易使用的扩展方法，就像 `@Html.BeginUmbracoForm` 一样。
* __@CurrentPage__ (对应类型 `DynamicPublishedContent`) -> 当前页面模型的动态表述，允许动态的操作字段以及动态 Linq。
* __@Model__ (对应类型 `Umbraco.Web.Mvc.RenderModel`) -> 视图对应的模型，包含一个叫`Content`的属性，可以让你操作当前页面的强类型 (对应类型`IPublishedContent`)。
* __@UmbracoContext__ (对应类型 `Umbraco.Web.UmbracoContext`)
* __@ApplicationContext__ (对应类型 `Umbraco.Core.ApplicationContext`)
* __@Members__ (对应类型 `Umbraco.Web.Security.MemberShipHelper`) [查看MemberShipHelper文档](../../Querying/MemberShipHelper/index.md)

## 使用UmbracoHelper 输出字段
这可能是用于输入当前内容条目的字段内容最常用的方法。

	@Umbraco.Field("bodyContent")

这有一些可选参数。这是它们的列表以及一些默认值：

* altFieldAlias = ""
* altText = ""
* insertBefore = ""
* insertAfter = ""
* recursive = false
* convertLineBreaks = false
* removeParagraphTags = false
* casing = RenderFieldCaseType.Unchanged
* encoding = RenderFieldEncodingType.Unchanged

使用 Field 方法的简单方式是直接指定设置你喜欢的可选参数。例如，如果我们想设置insertBefore 和insertAfter 参数，我们可以：

	@Umbraco.Field("bodyContent", insertBefore : "<h2>", insertAfter : "</h2>")


## 使用模型输出字段
UmbracoHelper方法提供了许多有用的参数来改变值的输出。但是如果你只是想简单的原样输出值，你可以使用视图的@Model.Content属性。例如：

	@Model.Content.Properties["bodyContent"].Value

或者:

	@Model.Content.GetPropertyValue("bodyContent")

你还可以给属性指定你想要的输出类型。如果属性编辑器或者值不支持指定的转换，则会引发异常。一些例子：

 	@Model.Content.GetPropertyValue<double>("amount")
	@Model.Content.GetPropertyValue<RawXElement>("xmlContents")

## 使用@CurrentPage (动态化)输出字段

UmbracoHelper方法提供了许多有用的参数来改变值的输出。但是如果你只是想简单的原样输出值，你可以使用视图的@CurrentPage属性。@CurrentPage和@Model.Content之间的不同在于，@CurrentPage是模型的动态表述，为查询暴露了许多动态特性。例如，输出字段你可以简单的使用语句：

	@CurrentPage.bodyContent

*注意: 如果你使用 Visual Studio 编辑你的模板，当你操作内容的动态语句时不会出现智能提示。*

## 输出宏

使用UmbracoHelper输出宏是非常简单的。这里有三个重载方法，我们从最基础的开始：

通过特定的别名，不包含任何参数输出一个宏：

	@Umbraco.RenderMacro("myMacroAlias")

通过使用匿名对象参数输出一个宏：

	@Umbraco.RenderMacro("myMacroAlias", new { name = "Ned", age = 28 })

通过使用字典对象参数输出一个宏

	@Umbraco.RenderMacro("myMacroAlias", new Dictionary<string, object> {{ "name", "Ned"}, { "age", 27}})


[UmbracoHelper 文档](../../Querying/UmbracoHelper/index.md)


## 访问会员数据

`@Members` 在模板化网站时是关联你的任何成员的接口。 [MemberShipHelper 文档](../../Querying/MemberShipHelper/index.md)

	@if(Members.IsLoggedIn()){
	   var profile = Members.GetCurrentMemberProfileModel();
	   var umbracomember = Members.GetByUsername(profile.UserName);
	   
	    <h1>@umbracomember.Name</h1>
	    <p>@umbracomember.GetPropertyValue<string>("bio")</p>
	}

## ModelsBuilder

ModelsBuilder 允许你在视图中使用强类型的模型。属性会在创建文档类型时创建，可以通过下面的语句操作：

	@Model.BodyText

When ModelsBuilder resolve your properties it will also try to use value converters to convert the values of your data into more convenient models allowing you to access nested objects as strong types instead of having to rely on dynamics and risking having a lot of potential errors when working with these.

[ModelsBuilder 文档](modelsbuilder.md)
