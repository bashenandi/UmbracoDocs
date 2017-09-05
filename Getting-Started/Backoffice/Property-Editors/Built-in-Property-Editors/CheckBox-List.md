#复选框列表

`返回值: 逗号分隔的字符串`

将预设值显示为复选框控件列表。保存的文本是一个逗号分隔的文本字符串。

注意: 不同于其他属性编辑器，不能在 Razor 中直接访问预设值

##数据类型定义示例

![True/Checkbox List Definition](images/wip.png)

##内容示例 

![Checkbox List Example](images/wip.png)

##MVC 视图示例

###强类型:

	@{
		if (Model.Content.HasValue("superHeros")){
			<ul>
			@foreach(var item in Model.Content.GetPropertyValue<string>("superHeros").Split(',')) {
				<li>@item</li>
			}
			</ul>
		}
	}

###动态: 

	@{
		if (CurrentPage.HasValue("superHeros")){
			<ul>
			@foreach (var item in CurrentPage.superHeros.Split(',')){
				<li>@item</li>
			}
			</ul>
		}
	}
    