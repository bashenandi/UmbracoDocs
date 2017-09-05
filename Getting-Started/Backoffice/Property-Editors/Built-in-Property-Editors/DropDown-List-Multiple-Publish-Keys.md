# 多选下拉列表，公开键值

`返回值: 逗号分隔的字符串`

根据预设值列表显示下拉列表，并且可以有多个值被选中。保存的值是逗号分隔的预设值 id 字符串。 

## 数据类型定义示例

![Dropdown list multiple Data Type Definition](images/wip.png)

## 内容示例

![Dropdown list multiple Content](images/wip.png)

##MVC 视图示例

###类型:
	
	@{
	    if (Model.Content.HasValue("superHeros"))
	    {
	        // this conditional is required becuase of a bug where a single item is returned as value instead of id
	        var superHeros = Model.Content.GetPropertyValue<string>("superHeros").Split(',');
	        <ul>
	            @if (superHeros.Count() == 1)
	            {
	                <li>@Model.Content.GetPropertyValue("superHeros")</li>
	            }
	            else
	            {
	                foreach (var item in superHeros.Select(int.Parse))
	                {
	                    <li>@Umbraco.GetPreValueAsString(item)</li>
	                }
	            }                                              
	        </ul>
	    }
	}

###动态:                              

	@{
	    if (CurrentPage.HasValue("superHeros"))
	    {
	        // this conditional is required becuase of a bug where a single item is returned as value instead of id
	        var superHeros = ((string)CurrentPage.superHeros).Split(',');
	        <ul>
	            @if (superHeros.Count() == 1)
	            {
	                <li>@CurrentPage.superHeros</li>
	            }
	            else
	            {
	                foreach (var item in superHeros.Select(int.Parse))
	                {
	                    <li>@Umbraco.GetPreValueAsString(item)</li>
	                }
	            }
	        </ul>      
	    }
	}
    