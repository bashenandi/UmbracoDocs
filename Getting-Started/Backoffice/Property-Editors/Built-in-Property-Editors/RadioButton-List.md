#单选按钮组

`返回值: 预设值 ID`

正如其名称所表明的，这个属性编辑器允许编辑器从单选按钮列表中进行选择。

##数据类型定义示例

![Radiobutton List Data Type Definition](images/wip.png)

##内容示例 

![Radiobutton List Content](images/wip.png)

##MVC 视图示例

###类型:

	@if (Model.Content.HasValue("miniFigure")){
		var preValue = Umbraco.GetPreValueAsString(Model.Content.GetPropertyValue<int>("miniFigure"));
		<p>@preValue</p>
	}

###动态:                              

	@if (CurrentPage.HasValue("miniFigure")){
		var preValue = Umbraco.GetPreValueAsString(CurrentPage.miniFigure);
		<p>@preValue</p>
	}   