# 输出媒体 #

_Templates(Views)可以操作[Media 库](../../Data/Creating-Media/index.md)中的条目，从而实现一些丰富的内容，例如相册_

在下面的示例中，我们将看到如何输出`image`，而且同样的原则适用于所有 MediaType 类型的条目，只是属性别名会有所不同。

## 输出单个媒体条目 ##
一个媒体节点并不是一个文件，就像内容一样，它是字段的集合，例如 width，height 以及存储的文件路径。这样做的好处是访问媒体与访问内容节点非常相似。

### 示例 1: 基于一个图片媒体的 ID 来操作 ###
媒体库中的标准图片，都是基于Mediatype`image`创建的，它提供了一些标准值 - 如果你想要添加更多，只需在**settings**下面编辑媒体类型。在这个例子中，我们要获取一个图片节点，使用媒体条目的 URL 来输出一个`img`标签，并使用 Name 作为它的`alt`属性。

_假设：我们假设媒体项目的 ID 为**1234**，并且我们**不使用Models Builder**_

    @{
        // 使用 TypedMedia 方法查询 id 为1234的图片对象
        var mediaItem = Umbraco.TypedMedia(1234);        

        // 获取媒体条目的 url，可以使用媒体条目的 URL 属性。
        var url = mediaItem.Url
    }

    <img src="@url" alt="@mediaItem.Name" />

但是请等一下，如果你使用的是 Umbraco v7.4.0以上的版本，它现在包含了[Models Builder](../../../Reference/Templating/Modelsbuilder/index.md)。这意味着，如果启用了（默认是开启的）Models Builder的话，你可以为媒体条目使用强类型来获取。

### 示例 2: 使用强类型的图片媒体条目来操作 ###
类似于示例1，我们预设了同样的 ID 来操作媒体类型`image`。

    @{
        // 我们可以使用OfType 扩展方法来转换IPublishedContent 类型
        // 通过TypedMedia 返回Models Builder实现
        var mediaItem = Umbraco.TypedMedia(1234).OfType<Image>();
    }

    <img src="@mediaItem.Url" height="@mediaItem.UmbracoHeight" />

**注意:**为了防止转换失败或者TypedMedia返回空值，对代码进行额外的空值检查是有必要的。这会使你的代码更加健壮，这是推荐的做法。

### 其他媒体项目例如 `File` ###
访问其他媒体项也可以以同样的方式，这些技术不仅限于“image”类型，而且是最常见的用例之一。

## Image Cropper ###

Image Cropper通常与`Image`媒体类型一起使用，因此在 Umbraco 中将其视为`Image` 媒体类型的上传属性是非常有用的。

如果您的媒体类型是针对图像的，并且其上传字段（umbracofile）为Image Cropper，则扩展方法`getcropurl `是非常有用的。Image Cropper属性编辑器的详细信息以及其他使用示例可以在[这里](../../Backoffice/Property-Editors/Built-in-Property-Editors/Image-Cropper.md)找到。下面的示例是一个快速示例来帮助你快速入门。

### 使用Image Cropper的Models Builder强类型`Image`示例 ###

    @{
        // 我们可以使用OfType 扩展方法来转换IPublishedContent  
        // 通过TypedMedia 返回Models Builder实现
        var mediaItem = Umbraco.TypedMedia(1234).OfType<Image>();
    }

    <img src="@mediaItem.GetCropUrl("myCropAlias")" />

本示例假设了你在你的Image Cropper数据类型中配置了一个裁剪属性叫**myCropAlias**。

如果你想要原始的，未裁剪的图片，你可以忽略GetCropUrl扩展方法，转而使用前面介绍过的方法，像下面这样。

    <img src="@mediaItem.Url" />

### 更多信息 ###
- [媒体选取器](../../Backoffice/Property-Editors/Built-in-Property-Editors/Media-Picker.md)
- [图片裁剪](../../Backoffice/Property-Editors/Built-in-Property-Editors/Image-Cropper.md)