#教程 - 创建一个属性编辑器

##概述
这篇指导讲述了如何设置一个简单的属性编辑器，如何关联到 Umbraco 的数据类型，如果连接到 angularjs 并注入，以及最终如何测试你的属性编辑器。

所以我们所有要操作的步骤是:

- 设置一个插件
- 编写基本的 "Hello World" HTML + JS
- 注册数据类型到 Umbraco
- 添加扩展依赖
- 完成 markdown 编辑器

##准备条件
这是关于如何在 Umbraco 中使用 AngularJS，因此并不包含 AngularJS 自身的内容，这里已经为你准备了一些资源：

- [egghead.io](http://www.egghead.io/)
- [angularjs.org/tutorial](http://docs.angularjs.org/tutorial)
- [Tekpub](http://tekpub.com/products/angular)

##最终结果

指导的最终，我们会有一个简单的 markdown 编辑器运行在 Umbraco 中，在后台注册为一个数据类型，用于文档类型，编辑器可以创建和修改数据。

##安装一个插件

第一件事，我们必须在`/App_Plugins`文件夹中创建一个新的文件夹。我们叫它`MarkDownEditor`。

接下来我们创建一个简单的manifest文件，来描述这个插件能干什么。这个 manifest 会告诉 Umbraco 关于我们的新属性编辑器以及允许我们注入任何需要的文件到应用中，因此我们创建这个文件`/App_Plugins/MarkDownEditor/package.manifest`，[完整的package.manifest JSON 文档看这里](../../Extending/Property-Editors/package-manifest.md)。

在这个打包manifest中，我们添加一些 JSON 来描述属性编辑器，查看下面 JSON 中的内联注释，了解每个详细信息
Inside this package manifest we add a bit of JSON to describe the property editor, have a look at the inline comments in the JSON below for details on each bit:

	{
		//你可以定义多个编辑器
		propertyEditors: [
			{
				/* 这必须是唯一的别名 */
				alias: "My.MarkdownEditor",
				/* 名字 */
				name: "My markdown editor",
				/* icon图标 */
				icon: "icon-code",
				/* "Select editor"对话框中的分组 */
				group: "Rich Content",
				/* 我们要为编辑器加载的 HTML 文件 */
				editor: {
					view: "~/App_Plugins/MarkDownEditor/markdowneditor.html"
				}
			}
		],
		/* 在应用启动时要注入到应用中的文件数组 */
		javascript: [
		    '~/App_Plugins/MarkDownEditor/markdowneditor.controller.js'
		]
	}


##编写一些基本的 HTML和 JS
然后我们添加两个文件到`/app_plugins/markdowneditor/`文件夹：
- `markdowneditor.html`
- `markdowneditor.controller.js`

这是我们编辑器的主要文件，.html 文件处理视图，.js 文件处理功能。

在.js 文件中，我们要添加基本的 AngularJS 控制器定义

	angular.module("umbraco").controller("My.MarkdownEditorController",
		function () {
			alert("The controller has landed");
		});

.html 文件中，我们添加：

	<div ng-controller="My.MarkdownEditorController">
		<textarea ng-model="model.value"></textarea>
	</div>

现在我们的编辑器的基本部分就完成了，即：
Now our basic parts of the editor is done, namely:

- 一个包装清单，告诉 Umbraco 要载入什么
- 用于编辑器的视图
- 控制器通过 angular 连接到编辑器。

##注册数据类型到 Umbraco
当上面的操作完成后，重启你的应用。前往开发区块，点击数据类型目录的三个点菜单，然后创建一个新的数据类型，名为"markdown"。在编辑器中，你现在可以选择一个编辑器，你新添加的"My markdown editor"会呈现出来。

保存数据类型，然后将它添加到某个你选择的文档类型中，打开这个文档类型，你会看到一个 alert 消息，说的是"The controller has landed"，这意味的一切都是正常的，现在就就可以在你的编辑器中编辑属性值了。

##添加扩展依赖
接下来继续深入一些，载入一个 markdown 编辑器 JS 库，我选择了[pagedown][1]，但是你也可以选择任何你想要的。

首先，我们要添加扩展文件到包的目录，位于`/app_plugins/markdowneditor/lib`文件夹，这些扩展文件可以从 pagedown 项目地址找到：[Pagedown-bootstrap on github.com][1]：

然后打开 `markdowneditor.controller.js`文件进行编辑，看起来如下面：

	angular.module("umbraco").controller("My.MarkdownEditorController",
		/* 注册assetsService服务 */
		function ($scope,assetsService) {
			/* 告诉assetsService去插件目录加载markdown.editor类库 */
			assetsService.load([
				"~/App_Plugins/MarkDownEditor/lib/markdown.converter.js",
				"~/App_Plugins/MarkDownEditor/lib/markdown.sanitizer.js",
				"~/App_Plugins/MarkDownEditor/lib/markdown.editor.js"
			]).then(function () {
				/* 这些功能将在依赖加载完成后执行 */
				alert("editor dependencies loaded");
			});
			
			/* 为编辑器加载独立的 css 避免它们阻断 js 的加载*/
			assetsService.loadCss("~/App_Plugins/MarkDownEditor/lib/markdown.css");
		});

这是在我们外部依赖中的加载，但是只会在编辑器需要时加载。

现在让我们用一些实例化pagedown 编辑器的代码来替换`alert()`：

	var converter2 = new Markdown.Converter();
	var editor2 = new Markdown.Editor(converter2, "-" + $scope.model.alias);
	editor2.run();

再给 HTML 中的富文本输入框添加 id，查看更多 HTML 的结构，请浏览 pagedown 的演示[这里](https://github.com/samwillis/pagedown-bootstrap/blob/master/demo/browser/demo.html):

	<div ng-controller="My.MarkdownEditorController" class="wmd-panel">
		<div id="wmd-button-bar-{{model.alias}}"></div>
		<textarea ng-model="model.value" class="wmd-input" id="wmd-input-{{model.alias}}">
			your content
		</textarea>
		<div id="wmd-preview-{{model.alias}}" class="wmd-panel wmd-preview"></div>
	</div>

现在，清除缓存，重启应用程序，重新加载文档，查看 pagedown 编辑器的运行。

当你保存或者发布时，编辑器的值会自动与当前内容对象同步并发送到服务器，这一些都是通过强大的angular和`ng-model`属性。

[下一步 - 添加配置到属性编辑器](part-2.md)


[1] https://github.com/samwillis/pagedown-bootstrap
