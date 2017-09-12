#教程 - 使用属性编辑器整合服务

##概述
这是属性编辑器教程的第三步。在这一部分，我们会整合一个 Umbraco 的内置服务。在这个示例中，我们会使用*dialog service*注入*media picker*，返回图片数据到 markdown 编辑器中。

##注入服务
首先，我们需要获取服务操作，这可以通过控制器的构造方法完成，我们在这里添加一个参数：

	angular.module("umbraco").controller("My.MarkdownEditorController",
		/* 注入 Umbraco 的assetsServce 和 dialogService */
		function ($scope,assetsService, dialogService) { ... }
	)

这和我们在第一步中添加*assetsService*是一样的。

##pagedown 中的钩子
我们使用的 pagedown 编辑器，有着比较友好的事件系统，所以我们可以容易地通过媒体选择在编辑器中设置事件触发钩子，要添加一个钩子，要在编辑器启动后：

	/* 启动编辑器 */
	var converter2 = new Markdown.Converter();
	var editor2 = new Markdown.Editor(converter2, "-" + $scope.model.alias);
	editor2.run();
	
	/* 定语图片对话框点击 */
	editor2.hooks.set("insertImageDialog", function (callback) {
		/* 这里我们可以拦截我们自己的对话框处理机制 */
		
		/* 告诉编辑器我们会自行获取并返回图片 url */
		return true;
	});

注意callback事件，这个callback用于返回任何想给编辑器的数据。

我们现在已经能够操作编辑器事件了，接下来通过`dialogService`触发一个媒体选择对话框。你可以注入你想要的 HTML 给这个服务，但是它也有一些固定方法，类似于媒体选择：

	/* 这个 callbak 会在选择了图片时调用 */
	dialogService.mediaPicker({callback: function(data){
		/* data.selection 包含了选择的图片数组，注意=====这会视具体版本不同而有所区别 */
		$(data.selection).each(function(i, item){
			/* 尝试使用$log.log(item)看看数据包含了什么 */
		});
	}});

##获取图片数据
由于 Umbraco 的特性，你并不知道图片具体在哪里，一个媒体对象数据是基于属性的数组，所以你如何选择正确的？你无法一直确定实例的属性名`umbracoFile`。

对于这样的情况，有一个可用的帮助服务：`imageHelper`。这个实用工具有助于获取嵌入在属性数据中的图像，以及相关的缩略图。**记住**在控制器构造函数中注入这个imageHelper（类似于dialogService和assetsService）。

现在我们就可以从选择的媒体条目中获取图片，并且通过 callback 返回：

	var imagePropVal = imageHelper.getImagePropertyValue({ imageModel: item, scope: $scope });
	callback(imagePropVal);

现在当我们运行 markdown 编辑器，并且点击图片按钮，我们会看到原生的 Umbraco 对话框，列出标准的媒体档案。

点击图片并选择返回编辑器的输出类似于：

	![Koala picture][1]

	  [1]: /media/1005/Koala.jpg

上面是正确的 markdown 代码，选择了图片，并且开启了预览，你会看到编辑器下面的图片。

##总结
通过之前的三个步骤，我们：

- 创建了一个插件
- 定义了一个编辑器
- 注入我们自己的 JS 库和第三方组件
- 使编辑器可配置
- 使用原生对话框和服务连接编辑器
- 浏览到选择的图片。

[下一步 - 给属性编辑器添加服务端代码](part-4.md)
