#Templating

_Umbraco 中的模板包含两个大的概念，称为模板和宏。模板用于你的网页 HTML 布局，而宏是可重用的动态组件，用于在你的模板中嵌入导航，表单，列表等。_

##模板
在 Umbraco 有两种可用的模板技术：

* MVC (views)
* WebForms (masterpages)

默认情况下 Umbraco 使用 MVC 模板。

####[使用MVC (views, razor, 等...)](Mvc/index.md)
描述了如何使用 MVC 视图，Razor 语句和可用的 API，以及如何创建表单的详细信息，一步一步的指导以及其他一些高级技术。

####[使用WebForms (masterpages, usercontrols, 等...)](Masterpages/index.md)
描述了如何使用 WebForms（masterpages）模板预计，以及它各种各样的组件，例如 UserControls 等...

如果你更喜欢使用 Webforms，你可以在*/Config/umbracoSettings.config*文件中改变默认的模板引擎，找到下面的部分并且设置为你想要使用的（*Mvc* or *WebForms*）：

	<templates>
		<defaultRenderingEngine>WebForms</defaultRenderingEngine>
	</templates>


####Hybrid 模板
Umbraco 能够同时使用 MVC 和 Webforms 模板，而默认你只能使用一种。Umbraco在检查一个 Webforms 母版页面文件之前会先会检查 MVC 视图文件。例如，如果如果在后台创建了一个名为"Home"的模板，然后当通过这个模板来输出内容时，Umbraco 会检查这些位置并且使用它发现的第一个文件：

~/Views/Home.cshtml
~/Masterpages/Home.master

##[宏](Macros/index.md)
描述了如何设置宏，使用宏参数以及配置缓存。定义不同类型的宏并且提供不同宏引擎的详细 API 信息以及用法。