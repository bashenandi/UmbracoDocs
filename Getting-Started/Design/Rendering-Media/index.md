#输出媒体

_模板可以操作媒体库中的条目，从而实现一些丰富的内容，例如相册_

##输出单个媒体条目
一个媒体节点不只是一个文件，而是字段的集合，例如 width，height 和存储文件的路径。

媒体库中的一个标准图片是基于媒体类型`image`的，提供了一些标准的值 - 如果你想添加更多，在 **settings** 下面编辑媒体类型。

	@{
		var mediaItem = Umbraco.Media(1234);
		var height = mediaItem.UmbracoHeight;
		var file = mediaItem.UmbracoFile;
	}
	<img src="@file" height="@height" />

##输出单个媒体的裁剪图片
如果媒体类型使用了 Image Cropper 替代了上传字段(umbracoFile)，你可以通过`GetCropUrl`方法，引用至裁剪器别名，来获取裁剪图片。

	@{
		var mediaItem = Umbraco.Media(1234);
		var croppedUrl = Url.GetCropUrl(mediaItem, "cropAlias");
	}
	<img src="@croppedUrl" />

如果你想要原始图片，而不是裁剪图片。

	@{
		var mediaItem = Umbraco.Media(1234);
		var file = mediaItem.umbracoFile.src;
	}
	<img src="@file" />

###更多信息
- [媒体选取器](../../Backoffice/Property-Editors/Built-in-Property-Editors/Media-Picker.md)
- [图片裁剪](../../Backoffice/Property-Editors/Built-in-Property-Editors/Image-Cropper.md)
