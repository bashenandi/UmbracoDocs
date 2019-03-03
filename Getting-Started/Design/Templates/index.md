# 模板 #

_Umbraco 中的模板，是建立在 asp.net MVC 中 Razor 视图之上的概念 - 如果你已经掌握这些，那就准备创建你的第一个模板吧 - 如果没有掌握，这也是一个快捷方便的指南。 _

## 创建你的第一个模板 ##
默认情况下，所有的文档类型都会有一个附属的模板 - 但在本例中，你需要一个可替换的模板或一个新的模板，你可以创建一个：

在 Umbraco 后台打开_Settings_板块，右键右击 **templates** 文件夹后选择 __Create__ 。输入模板的名字并点击 create 按钮。你会看到默认的模板编辑已经在后台的模板编辑器中了

![创建模板](images/create-template.png)


## 允许文档类型可以使用的模板 ##
若要在文档上使用模板，必须首先在内容类型上允许使用模板。打开想要使用这个模板的文档类型，然后在"template"标签下的"Allowed templates"中选择添加。

![允许模板](images/allow-template.png)

## 继承主模板 ##
通过使用 asp.net 的视图特性，一个模板可以从主模板中继承内容。假设我们有一个叫**masterview**的模板，包含下面的 html：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
		Layout = null;
	}
	<!DOCTYPE html>
	<html lang="en">
		<body>
			<h1>Hello world</h1>
			@RenderBody()
		</body>
	</html>

然后我们创建一个叫**textpage**的新模板，然后进入模板编辑器，在**properties**标签页中，设置它的 *Master template* 为**MasterView** ：

![继承模板](images/inherit-template.png)

这实际上是改变了模板中的`Layout`，所以 **textpage** 看起来是这样的：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
		Layout = "MasterView.cshtml";
	}
	<p>My content</p>

当一个页面使用textpage输出时，最终的 html 会合并替换`@renderBody()`占位符，最终结果为：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
		Layout = null;
	}
	<!DOCTYPE html>
	<html lang="en">
		<body>
			<h1>Hello world</h1>
			<p>My content</p>
		</body>
	</html>

## 模板区块 ##
要知道的是，你并不局限于只能使用一个区块。模板区块允许继承自主模板布局的子模板，可以插入一些 html 代码到主模板布局中。例如子模板插入一些代码到主模板的`<head>`标签中。

你可以通过使用命名区块来实现。使用如下的代码在主模板中添加命名区块：

	@RenderSection("SectionName")

例如，如果你想添加 HTML 标签到你的`<head>`标签，编写：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
		Layout = null;
	}
	
	<html>
		<head>
			<title>Title</title>
			@RenderSection("Head")
		</head>
		
		<body>
		</body>
	</html>

默认情况下，当定义一个区块时它是必填的。将区块设置为可选，请添加`required:false`。

	@RenderSection("Head", required: false)

在你的子页面中调用`@section Head {}`，然后输入你的标记，它就会显示在主模板中。

    @section Head {
        <style>
            body {
                background: #ff0000;
            }
        </style>
    }

## 注入局部视图 ##
另外一个重用 html 的方法是适应局部视图 - 这是一种很小的可重用视图，可以注入到其他视图中。
像模板一样，创建局部视图，通过点击"partial views"然后选择"create" - 你还可以选择预定义模板。

![创建局部视图](images/create-partial.png)

创建好的局部视图，可以通过使用`@Html.Partial()`方法注入到任何模板中，像这样：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
		Layout = "MasterView.cshtml";
	}
	
	<h1>My new page</h1>
	@Html.Partial("a-new-view")

### 更多信息:

- [基本Razor语法](basic-razor-syntax.md)
- [输出内容](../Rendering-Content/)

### 教程
- [使用 Umbraco 创建一个基础网站](../../../Tutorials/Creating-Basic-Site/)

### [Umbraco.TV](https://umbraco.tv)
- [Chapter: Templating](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/templating/introduction/)