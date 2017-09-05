# 内容选取器 #

`别名: Umbraco.ContentPicker2`

`返回: IPublishedContent`

内容选取器打开一个面板，用于从内容结构中选择指定的页面。这个值保存的是选择节点的 UDI。

## 数据类型定义示例

![Content Picker Data Type Definition](images/Content-Picker2-DataType.png)

## 内容示例 

![Content Picker Content](images/Content-Picker2-Content.png)

## 类型示例: ##

	@{
		IPublishedContent typedContentPicker = Model.Content.GetPropertyValue<IPublishedContent>("featurePicker");
		if (typedContentPicker != null){
			<p>@typedContentPicker.Name</p>
		}
	}