<<<<<<< HEAD
# 多选下拉列表
=======
# (Obsolete) Dropdown list multiple
>>>>>>> upstream/master

`返回值: 逗号分隔的字符串`

<<<<<<< HEAD
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
    
=======
Displays a list of preset values as a list where multiple values can be selected. The value saved is a comma separated string of the text values.

## Data Type Definition Example

![Dropdown list multiple Data Type Definition](images/wip.png)

## Content Example

![Dropdown list multiple Content](images/wip.png)

## MVC View Example

### Typed

```csharp
@{
    if (Model.Content.HasValue("superHeros")){
        <ul>
        @foreach(var item in Model.Content.GetPropertyValue<string>("superHeros").Split(',')) {
        <li>@item</li>
        }
    </ul>
    }
}
```

### Dynamic

```csharp
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
```
>>>>>>> upstream/master
