# 相关链接

`返回值: RelatedLinks`

相关链接允许编辑器简易的将链接添加为数组。可以包含 Umbraco 内部链接以及外部URLs。

##  数据类型定义示例

![Related Links Data Type Definition](images/Related-Links2-DataType.png)

## 内容示例 

![Media Picker Content](images/Related-Links2-Content.png)

## MVC 视图示例 - [启用值转换器](../../../Setup/Upgrading/760-breaking-changes.md#property-value-converters-u4-7318)

### 类型:

	@using Umbraco.Web.Models
	@{
		var typedRelatedLinksConverted = Model.Content.GetPropertyValue<RelatedLinks>("footerLinks");
		if (typedRelatedLinksConverted.Any()){
			<ul>
				@foreach (var item in typedRelatedLinksConverted){
					var linkTarget = (item.NewWindow) ? "_blank" : null;
					<li><a href="@item.Link" target="@linkTarget">@item.Caption</a></li>
				}
			</ul>
		}
	}