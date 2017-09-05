# 是/非

`返回值: Boolean`

是/非是一个简单的复选框，保存值是0或1，这取决于你是否选取了。

## 数据类型定义示例

![True/False Data Type Definition](images/True-False-DataType.png)

## 内容示例 

![No Edit Content Example](images/True-False-Content.png)

##MVC 视图示例 - 显示子页面中没有隐藏的链接列表

###类型:

	@{
		foreach (IPublishedContent page in Model.Content.Children){
			if (!page.GetPropertyValue<bool>("umbracoNaviHide")){
				<p>@page.Name</p>
			}
		}	
	}

###动态: 

	@{
		foreach (var page in CurrentPage.Children){
			if (!page.umbracoNaviHide){
				<p>@page.Name</p>
			}
		}	
	}