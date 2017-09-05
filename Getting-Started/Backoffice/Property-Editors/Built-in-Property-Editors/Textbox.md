#文本框

`返回值: String`

文本框是用于简单文本的输入控件

##数据类型定义示例

![Textbox Data Type Definition](images/Textbox-DataType.png)


##内容示例 

![Textbox Content Example](images/Textbox-Content.png)

##MVC 视图示例

###类型:

	@{
	   if (Model.Content.HasValue("pageTitle")){
	       <p>@Model.Content.GetPropertyValue<string>("pageTitle")</p>
	   } 
	}

###动态: 

	@{       
	   if (CurrentPage.HasValue("pageTitle")){
	       <p>@CurrentPage.pageTitle</p>
	   } 	       
	}
