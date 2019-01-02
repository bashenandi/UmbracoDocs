# 局部视图宏

_局部视图宏是推荐在 Umbraco 中使用的宏类别。它们可以运行在 MVC 和 WebForms 中，通过`UmbracoHelper`使用统一的查询语法_

## 视图/模型 类型

所有的局部视图宏的视图都继承自`Umbraco.Web.Macros.PartialViewMacroPage`，因此每个局部视图宏文件的头部都有下面的语句：

	@inherits Umbraco.Web.Macros.PartialViewMacroPage
局部视图宏的模型类型是`Umbraco.Web.Models.PartialViewMacroModel`，包含所有你会需要的用来输出内容的属性以及宏文件自身的附加属性：`MacroName`, `MacroAlias`, `MacroId`, 以及 `MacroParameters`。

## 文件信息

默认情况局部视图宏存储在这个文件夹：

> ~/Views/MacroPartials 

如果你是作为包的一部分捆绑了局部视图宏，他们也能存在于下面文件夹：

> ~/App_Plugins/[YourPackageName]/Views/MacroPartials

其实局部视图宏也只是一个正常的 MVC 局部视图，它们的文件扩展名都是**cshtml**。所有的局部视图宏都继承自视图类

	Umbraco.Web.Macros.PartialViewMacroPage

因此所有文件都会包含这个头（当你在 Umbraco 后台中创建局部视图宏时会自动完成）：

	@inherits Umbraco.Web.Macros.PartialViewMacroPage

## 访问内容

局部视图宏中的语句100%和**[MVC 视图](../../Mvc/views.md)中的语句一致，事实上它们都是通过 MVC 视图引擎驱动的。

你可以使用@CurrentPage, @Model.Content, @Umbraco，等等

## 访问宏参数

你可以通过模型属性中类型为`IDictionary<string, object>`的`MacroParameters`来操作宏的参数

    var myParam = Model.MacroParameters["aliasOfTheMacroParameter"];
