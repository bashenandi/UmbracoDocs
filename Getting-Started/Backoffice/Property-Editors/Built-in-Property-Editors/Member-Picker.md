
# 会员选取器

`返回: 会员 ID`

会员选取器会打开面板，从会员区块中选取特定的会员。这个值保存的是选中会员的 ID。

## 数据类型定义示例

![Media Picker Data Type Definition](images/Member-Picker-DataType.png)

## 内容示例

![Memebr Picker Content](images/Member-Picker-Content.png)

##MVC 视图示例

###类型:

# Member Picker

`Returns: Member ID`

The member picker opens a panel to pick a specific member from the member section. The value saved is the selected member ID.

## Data Type Definition Example

![Media Picker Data Type Definition](images/Member-Picker-DataType.png)

## Content Example 

![Member Picker Content](images/Member-Picker-Content.png)

## MVC View Example

### Typed:

	@{
		if(Model.Content.HasValue("author")){
			var member = Umbraco.TypedMember(Model.Content.GetPropertyValue<int>("author"));
			@member.Name
		}
	}

###动态:  
				@member.Name
		}
	}

### Dynamic (Obsolete):

See [Common pitfalls](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics) for more information about why the dynamic approach is obsolete.

	@{
		if(CurrentPage.HasValue("author")){
			var member = Umbraco.TypedMember(CurrentPage.author);
			@member.Name
		}
	}
	
				@member.Name
		}
	}