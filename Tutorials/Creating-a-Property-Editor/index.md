# 教程 - 创建一个属性编辑器 #

## 概述 ##

本指南介绍了如何设置一个简单的属性编辑器，如何将其关联到 Umbraco 的数据类型，如何将其关联到 Angular's模块并注入，以及最后如何测试你的属性编辑器。

以下是我们所要操作的所有步骤：

- 设置一个插件
- 编写基本的 "Hello World" HTML 和 JS
- 在 Umbraco 中注册数据类型
- 添加外部依赖
- 完成 markdown 编辑器

## 先决条件 ##

这里仅包含了如何在 Umbraco 中使用 AngularJS，并不包含 AngularJS 自身的内容，这里我们为你准备了一些资源：

- [egghead.io](https://egghead.io/courses/angularjs-fundamentals)
- [angularjs.org/tutorial](https://docs.angularjs.org/tutorial)
- [Pluralsight](https://www.pluralsight.com/paths/angular-js)

## 最终结果 ##

本指南结束时，我们将会得到一个在 Umbraco 后台中注册为数据类型的简单 markdown 编辑器，可以分配给文档类型，编辑可以创建和修改数据。


##安装一个插件

首先第一件事情，我们必须在`/App_Plugins`文件夹中创建一个新的文件夹。我们叫它`MarkDownEditor`。然后我们要创建一个简单的manifest文件，来描述这个插件是做什么的。这个 manifest 会告诉 Umbraco 我们新的属性编辑器相关的信息，同时允许我们将任何需要的文件注入到应用中，因此我们创建文件`/App_Plugins/MarkDownEditor/package.manifest`，[完整的package.manifest JSON 文档看这里](../../Extending/Property-Editors/package-manifest.md)。

在这个包的manifest文件中，我们添加一些 JSON 来描述这个属性编辑器。有关每个节点的详细信息，请查看下面 JSON中的内联注释：

	{
		//你可以定义多个编辑器
		propertyEditors:[
			{
				/* 必须唯一的别名 */
				alias: "My.MarkdownEditor",
				/* 显示名字 */
				name: "My markdown editor",
				/* icon图标，由 Umbraco 系统内部支持 */
				icon: "icon-code",
				/* "Select editor"对话框中的分组 */
				group: "Rich Content",
				/* 为编辑加载的 HTML 文件 */
				editor: {
					view: "~/App_Plugins/MarkDownEditor/markdowneditor.html"
				}
			}
		],
		//在应用启动时需要注入到应用中的文件的数组数组
		javascript:[
			'~/App_Plugins/MarkDownEditor/markdowneditor.controller.js'
		]
	}

## 编写一些基本的 HTML和 JS ##

接下来我们添加下面两个文件到`/app_plugins/markdowneditor/`文件夹：

- `markdowneditor.html`
- `markdowneditor.controller.js`

这些是我们用于编辑器的主要文件，.html 文件处理视图，.js 文件处理功能。

在.js 文件中，我们要添加基本的 AngularJS 控制器定义

	angular.module("umbraco").controller("My.MarkdownEditorController",
		function () {
			alert("The controller has landed");
		});
在.html 文件中，我们添加：

	<div ng-controller="My.MarkdownEditorController">
		<textarea ng-model="model.value"></textarea>
	</div>

现在我们编辑器的基本部分就完成了，换种方式来说：

- 一个包装清单，告诉 Umbraco 要加载什么
- 用于编辑器的视图
- 控制器通过 angular 连接到编辑器

## 注册数据类型到 Umbraco ##
完成上面的操作后，重启你的应用。前往"Developer"板块，点击"Data Types"目录右边的三个点，创建一个新的数据类型，命名为"markdown"。在编辑器中，你可以选择一个"Property editor"，刚刚新建的"My markdown editor"会出现在属性编辑器中。


保存数据类型，然后将它添加到某个你选择的文档类型中。打开这个类型的文档，你会收到一个alert 消息说："The controller has landed"，这意味的一切都是正常的。现在你就可以使用编辑器编辑指定属性的值。

## 添加外部依赖 ##

让我们深入下去，载入一个 markdown 编辑器 JS 库，我选择了[pagedown][1]，当然你也可以选择任何你想要的。

首先，我们要添加一些外部文件到包的目录，即`/app_plugins/markdowneditor/lib`文件夹。这些外部文件可以从 pagedown 的项目地址找到：[Pagedown-bootstrap on github.com][1]：

接下来打开 `markdowneditor.controller.js`文件进行编辑，如下所示：

	angular.module("umbraco").controller("My.MarkdownEditorController", 
	//注入 Umbraco 的assetsService
	function($scope, assetsService){
		/* 告诉assetsService从markdown编辑器的插件目录中加载 markdown.editor类库 */
		assetsService.load([
			"~/App_Plugins/MarkDownEditor/lib/markdown.converter.js",
			"~/App_Plugins/MarkDownEditor/lib/markdown.sanitizer.js",
			"~/App_Plugins/MarkDownEditor/lib/markdown.editor.js"
		]);
	
		assetsService.load([
			"~/App_Plugins/MarkDownEditor/lib/markdown.converter.js",
			"~/App_Plugins/MarkDownEditor/lib/markdown.sanitizer.js",
			"~/App_Plugins/MarkDownEditor/lib/markdown.editor.js"
		])
		.then(function(){
			/* 这些方法会在所以依赖加载完成后执行 */
			alert("editor dependencies loaded");
		});
		
		/* 为编辑器加载独立的 css 避免它们阻断 js 的加载*/
		assetsService.loadCss("~/App_Plugins/MarkDownEditor/lib/markdown.css");
	});

这些外部依赖的加载，只会在编辑器需要它们时被加载。

现在让我们用一些代码来替换`alert()`，使得pagedown编辑器可以被实例化：

	var converter2 = new Markdown.Converter();
	var editor2 = new Markdown.Editor(converter2, "-" + $scope.model.alias);
	editor2.run();

给html 中的 textarea 添加它的 id。更多关于 HTML 的结构信息，请浏览 pagedown 的演示[这里](https://github.com/samwillis/pagedown-bootstrap/blob/master/demo/browser/demo.html):

	<div ng-controller="My.MarkdownEditorController" class="wmd-panel">
		<div id="wmd-button-bar-{{model.alias}}"></div>
		<textarea ng-model="model.value" class="wmd-input" id="wmd-input-{{model.alias}}">
			your content
		</textarea>
		<div id="wmd-preview-{{model.alias}}" class="wmd-panel wmd-preview"></div>
	</div>

现在，清除缓存，重新加载文档，看看 pagedown 编辑器的运行状况。

当你保存或者发布时，编辑器的值会自动与当前内容对象同步并发送到服务器，这些都是通过强大的angular和`ng-model`属性实现的。

[下一步 - 给属性编辑器添加配置项](part-2.md)


[1] https://github.com/samwillis/pagedown-bootstrap
