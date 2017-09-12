#教程 - 添加配置到属性编辑器

##概述
这是我们创建属性编辑器指导的第二步。这一步继续工作在我们[第一步](creating-tutorial1-v7.md)创建的 markdown 编辑器中，但是进一步呈现给你如何给编辑器添加一些配置项。


##配置?
建立一个好的属性编辑器的重要部分，是构建相对灵活的东西，你可以在不同的时候，多次的重复使用它；就像 Umbraco 中的富文本编辑器，它允许你为每个编辑器实例，选择想要的按钮和样式表。

因此一个编辑器能够重复使用，具有不同的配置，是现在我们要完成的工作。


##package.manifest
要给我们的 markdown 编辑器添加配置属性，再次打开`package.manifest`文件。在编辑器定义的下面，粘贴下面的代码：

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

我们添加了什么？我们添加了一个预设值编辑器，具有`fields`集合。这个集合包含我们的这个编辑器想要输出数据类型配置的相关信息。

首先取到的是"Preview"，使用"boolean"视图，因此这会提供给用户一个简单的复选框，允许切换预览开启/关闭。"boolean"来自于转换器，所有的编辑器预览都存储在`/umbraco/views/prevalueeditors/`，通过`<name>.html`查找。

下一个也相同，只是它提供一个文本域输入框给用户，用于给编辑器设置默认值。

保存清单，**重启应用程序池**，现在再看一下 Umbraco 中嗯 markdown 数据类型。你会看到有了两个配置项。

##使用配置
下一步是访问操作新的配置项。打开`markdowneditor.controller.js`文件。

首先添加默认值功能。基本上，当 `$scope.model.value` 为空或者*undefined*时，我们想要使用默认值，因此我们在控制器的头部添加下面的代码：

	if($scope.model.value === null || $scope.model.value === ""){
		$scope.model.value = $scope.model.config.defaultValue;
	}

你新发现了什么？ - 是`$scope.model.config`对象。另一件你会注意到的事，是我们现在操作的`$scope.model.config.defaultValue`，包含了这个 key 所定义的配置值，它可以简单的安装以及从代码中访问这些配置值。

你可以不通过任何 JS 来使用这些值，打开`markdowneditor.html`。

因为我们可以直接在 HTML 中使用这些配置，就像下面一样，我们使用它来切换预览`<div>`，通过`ng-hide`属性：

	<div ng-show="model.config.preview" class="wmd-panel wmd-preview"></div>


[下一步 - 使用属性编辑器整合服务](part-3.md)
