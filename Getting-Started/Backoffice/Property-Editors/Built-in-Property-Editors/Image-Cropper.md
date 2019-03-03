# 图片裁剪 #

`返回值: JSON`

返回图像的路径，以及有关焦点和裁剪的信息。

## 设置 ##

### 预设值 ###
你可以添加、编辑和删除可以使用的裁剪 UI 设置项。

## 数据类型定义示例 ##

![Image Cropper Data Type Definition](images/Image-Cropper/datatype.png)

## 内容示例  ##
提供上传图片的 UI，设置图片上的焦点，使用预设的裁剪属性进行裁剪和缩放。所以默认情况下，裁剪框中的图片只显示基础的焦点，和已有的特殊裁剪，如果他们存在的话。

裁剪有三个步骤：

- 上传图片
- 设置焦点
- 裁剪图片为预设裁剪值

### 裁剪图片 ###
编辑器会显示一个简单的文件拖拽区域。点击上传图片。
![Image Cropper Upload](images/Image-Cropper/upload.png)

### 设置焦点 ###
默认情况下，裁剪器允许编辑器在上传图片上设置焦点。
在下图中，所有预设的裁剪值，都会给编辑器提供预览图片，显示出呈现给最终用户的样子。

![Image Cropper Focal point](images/Image-Cropper/focalpoint.png)

### 裁剪和缩放 ###
如果需要的话，编辑器可以将图片裁剪为特定的预设尺寸，确认正确的区域和尺寸会显示特定的裁剪尺寸。

![Image Cropper Crop](images/Image-Cropper/crop.png)


## 示例代码 ##

图片裁剪，提供了一个简单的 API 用于生成裁剪图片的 URL，或者你可以直接操作原始数据指向为动态对象。

在 Umbraco v7.3.5中，UrlHelper扩展方法替代了 IPublishedContent 扩展方法。

### MVC 视图示例，从别名"image"属性中获取裁剪对象，并输出 "banner"裁剪值 ###

#### 类型 (Umbraco v7.3.5+): ####

    <img src="@Url.GetCropUrl(Model.Content, "image", "banner")" />

#### 类型 (低于 Umbraco v7.3.5): ####

    <img src="@Model.Content.GetCropUrl("image", "banner")" />

#### 动态 (Obsolete): ####
    
    // show the crop preset "banner"
    <img src="@CurrentPage.GetCropUrl("image", "banner")" />

    // or from specific node:
    <img src="@Umbraco.Media(1234).GetCropUrl("image", "banner")" />

### MVC 视图示例输出创建的指定尺寸裁剪图 - 在这里是一张 300 x 400 px 的图片 ###

#### 类型 (Umbraco v7.3.5+):
    
	@if (Model.Content.HasValue("image"))
    {
        <img src="@Url.GetCropUrl(Model.Content, propertyAlias: "image", height: 300, width: 400)" />
    }

#### 类型 (pre Umbraco v7.3.5):
    
    @if (Model.Content.HasValue("image"))
    {
        <img src="@Model.Content.GetCropUrl(propertyAlias: "image", height: 300, width: 400)" />
    }


### Media 示例 - 从属性别名"umbracoFile"中输出"banner"裁剪 ###

媒体对象的裁剪结果 URL 可以用类似的方法获得：

#### 类型 (Umbraco v7.3.5+): ####
    
    @{
        var mediaItem = Umbraco.TypedMedia(1234);
        if (mediaItem != null)
        {
            <img src="@Url.GetCropUrl(mediaItem, "banner")" />
        }
    }

#### 类型 (pre Umbraco v7.3.5): ####

    @Umbraco.TypedMedia(1234).GetCropUrl("banner")

#### Dynamic (Obsolete): ####

    @Umbraco.Media(1234).GetCropUrl("banner")


### CSS 背景示例 - 从属性别名"umbracoFile"中输出"banner"裁剪

从Umbraco v7.3.5 开始有一个选填参数"htmlEncode"，你可以指定为 false，则 Url 不会被 Html encode。

#### 类型 (Umbraco v7.3.5+): ####

    @{
        var mediaItem = Umbraco.TypedMedia(1234);
        if (mediaItem != null)
        {
            <style>
                .myCssClass {
                    background-image: url("@Url.GetCropUrl(mediaItem, "banner", false)");
                }
            </style>
        }
    }

### 数据返回值 ###

裁剪对象返回一个动态对象，基于 json 格式，类似于：

	{
		"focalPoint": {
			"left": 0.23049645390070922,
			"top": 0.27215189873417722
		},
		"src": "/media/SampleImages/1063/pic01.jpg",
		"crops": [
			{
				"alias": "banner",
				"width": 800,
				"height": 90
			},
			{
				"alias": "highrise",
				"width": 80,
				"height": 400
			},
			{
				"alias": "thumb",
				"width": 90,
				"height": 90
			}
		]
	}

所以通过动态操作属性直接获取：

    <img src='@CurrentPage.image.src'/>

或者遍历它们：

	@foreach(var crop in CurrentPage.image.crops){
		<img src="@CurrentPage.GetCropUrl("image", crop.alias)">
	}

## 由 ImageProcessor 提供 ##
[ImageProcessor](http://imageprocessor.org/)是一个神奇的项目，使用简单，但是可以高效的编辑处理图片。

我们在 Umbraco 7.1以上的版本引用了它，所以你可以开箱即用，使用它的全部功能，例如：sharping、blurring、cropping、rotating等等。

### MVC 视图示例 - 如何模糊裁剪对象 ###

#### 类型 (Umbraco v7.3.5+): ####

    <img src="@Url.GetCropUrl(Model.Content, propertyAlias: "image", cropAlias: "banner", useCropDimensions:true, furtherOptions: "&blur=11&sigma=1.5&threshold=10")" />
    
#### 动态: ####

    <img src='@CurrentPage.GetCropUrl("image", "banner")&blur=11&sigma=1.5&threshold=10' />


## 上传属性替代品 ##

你可以使用裁剪替代上传属性，已有的图片依旧返回当前路径，应用裁剪后的修改。旧的图片在裁剪器中是可用的，因此如果需要的话你可以修改它们。

然而，要知道裁剪器在保存时返回的时动态对象，所以如果你的上传属性中修改了排序或者字符串属性，可能会导致在模板/宏中发生一些错误。