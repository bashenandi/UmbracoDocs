# 多选下拉列表

`返回值: 逗号分隔的字符串`

根据预设值列表显示下拉列表，并且可以有多个值被选中。保存的值是逗号分隔的文本字符串。

## 数据类型定义示例

![Dropdown list multiple Data Type Definition](images/wip.png)

## 内容示例

![Dropdown list multiple Content](images/wip.png)

##MVC 视图示例

###类型:
	
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
	    if (CurrentPage.HasValue("superHeros"))
	    {
	        <ul>
	            @foreach (var item in CurrentPage.superHeros.Split(','))
	            {
	                <li>@item</li>
	            }
	        </ul>
	    }
	}
    