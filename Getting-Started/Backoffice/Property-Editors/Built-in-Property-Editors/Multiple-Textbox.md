#多文本输入框

`返回: 字符串数组`

多文本框属性编辑器允许在内容编辑器中生成一组文本条目的列表。最好使用无序列表。

##数据类型定义示例

![Multiple Textbox Data Type Definition](images/wip.png)

##内容示例 

![Multiple Textbox Content](images/wip.png)

##MVC 视图示例

###Typed:


	@{
		if (Model.Content.GetPropertyValue<string[]>("keyFeatureList").Length > 0){
			<ul>
				@foreach (var item in Model.Content.GetPropertyValue<string[]>("keyFeatureList")){
					<li>@item</li>
				}
			</ul>
		}
	}

###Dynamic:                              


	@{
		if (CurrentPage.keyFeatureList.Length > 0){
			<ul>
				@foreach (var item in CurrentPage.keyFeatureList){
					<li>@item</li>
				}
			</ul>
		}
	}