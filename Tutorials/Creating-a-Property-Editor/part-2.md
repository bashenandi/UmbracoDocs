#教程 - 给属性编辑器添加配置项

## 概述 ##

这是我们创建一个属性编辑器指南的第二步。这一步继续在我们[第一步](./)创建的 markdown 编辑器上进行工作，将进一步教会你如何给编辑器添加一些配置项。

## 配置? ##
构建好的属性编辑器的一个重要部分，是构建相对灵活的内容，让你可以多次的将其用于不同的内容；就像 Umbraco 中的富文本编辑器，它允许你为每个编辑器实例，选择想要使用的按钮和样式表。

因此一个编辑器可以通过不同的配置，被多次重复使用，就是我们现在要进行的工作。

## package.manifest ##

打开`package.manifest`文件给我们的markdown 编辑器添加配置选项。在编辑器定义的下方，粘贴以下内容：

	prevalues: {
		fields: [
			{
				label: "Preview",
				description: "Display a live preview",
				key: "preview",
				view: "boolean"
			},
			{
				label: "Default value",
				description: "If value is blank, the editor will show this",
				key: "defaultValue",
				view: "textarea"
			}
		]
	}
	
**记住:** 分隔编辑器元素和编辑器预设值定义是通过逗号，你可以会遇到 JSON格式错误提示。

我们添加了什么？我们添加了一个编辑器预设值，含有`fields`集合。这个集合包含了我们想要在这个编辑器渲染配置时显示的数据类型 UI 相关信息。

首先获取到的标签是"Preview"，使用了"boolean"视图，这会提供给用户一个简单的复选框，允许切换预览开启/关闭。"boolean"来自于编辑器预览转换器，所有的预览编辑器都存储在`/umbraco/views/prevalueeditors/`，通过`<name>.html`查找。

下一个配置也相同，只是它提供一个文本域输入框给用户，用于给编辑器设置默认值。

保存清单，**重启应用程序池**，再看一下 Umbraco 中的 markdown 数据类型。现在你会看到多出了两个配置选项。

## 使用配置 ##

下一步是操作我们新的配置选项。打开`markdowneditor.controller.js`文件。

让我们先添加默认值的功能。一般来说，当 `$scope.model.value` 为空或者*undefined*时，我们想要使用默认值，因此我们在控制器的最开头添加下面的代码：

	if($scope.model.value === null || $scope.model.value === ""){
		$scope.model.value = $scope.model.config.defaultValue;
	}

有什么新发现麽？ - 是`$scope.model.config`对象。另一件你该注意到的事情，是我们现在所读取的`$scope.model.config.defaultValue`，包含了这个 key 的配置值，它可以简单的从代码中配置和使用这些配置值。

而且，你还可以不通过任何JabaScript 来使用这些值，打开`markdowneditor.html`进行替换。

因为我们可以直接在 HTML中使用这些配置，例如可以使用`ng-show`属性来切换预览`<div>`：

	<div ng-show="model.config.preview" class="wmd-panel wmd-preview"></div>


[下一步 - 属性编辑器整合服务](part-3.md)
