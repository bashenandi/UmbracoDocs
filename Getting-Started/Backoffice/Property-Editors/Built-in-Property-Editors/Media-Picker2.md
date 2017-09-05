# 媒体选取器 #

`别名: Umbraco.MediaPicker2`

`返回值: IEnumerable<IPublishedContent>` or `IPublishedContent`

这个属性编辑器，当数据类型设置"Pick multiple items"禁用时返回单独的条目，启用时返回一个集合。

## 数据类型定义示例

![Media Picker Data Type Definition](images/Media-Picker2-DataType.png)

## 内容示例 

![Media Picker Content](images/Media-Picker2-Content.png)

## 类型示例 (启用多选): ##

	@{
		var typedMultiMediaPicker = Model.Content.GetPropertyValue<IEnumerable<IPublishedContent>>("sliders");
		foreach (var item in typedMultiMediaPicker){
			<img src="@item.Url" style="width:200px"/>
		}
	}


## 类型示例 (禁用多选): ##

	@{
		var typedMediaPickerSingle = Model.Content.GetPropertyValue<IPublishedContent>("featuredBanner");
		if (typedMediaPickerSingle != null){
			<p>@typedMediaPickerSingle.Url</p>
			<img src="@typedMediaPickerSingle.Url" style="width:200px" alt="@typedMediaPickerSingle.GetPropertyValue("alt")" />
		}
	}      
