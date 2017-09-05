# 会员选取器

`返回: 会员 ID`

会员选取器会打开面板，从会员区块中选取特定的会员。这个值保存的是选中会员的 ID。

## 数据类型定义示例

![Media Picker Data Type Definition](images/Member-Picker-DataType.png)

## 内容示例

![Memebr Picker Content](images/Member-Picker-Content.png)

##MVC 视图示例

###类型:

	@{
		if(Model.Content.HasValue("author")){
			var member = Umbraco.TypedMember(Model.Content.GetPropertyValue<int>("author"));
			@member.Name
		}
	}

###动态:                              

	@{
		if(CurrentPage.HasValue("author")){
			var member = Umbraco.TypedMember(CurrentPage.author);
			@member.Name
		}
	}
	
