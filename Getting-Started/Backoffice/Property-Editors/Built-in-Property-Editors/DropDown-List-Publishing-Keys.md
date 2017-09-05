# 下拉列表, 公开键值

`返回值: Int`

根据预设值返回列表。保存值为选中值的 id。

## 设置

### 预设值
你可以添加、编辑和删除预设值的数据，以显示在下拉列表中。

## Data Type Definition Example

![Dropdown List Data Type Definition](images/Dropdown-List-Keys-DataType.png)

## Content Example 

![Downdown List Content](images/Dropdown-List-Keys-Content.png)

## MVC 视图示例输出选中值

### 类型:

    @if (Model.Content.HasValue("character"))
    {        
        var preValue = Umbraco.GetPreValueAsString(Model.Content.GetPropertyValue<int>("character"));
        <p>@preValue</p>
    }

### 动态:     
                         
    @if (CurrentPage.HasValue("character"))
    {
        var preValue = Umbraco.GetPreValueAsString(CurrentPage.character);
        <p>@preValue</p>        
    }    

## MVC 视图示例列出所有包含下拉列表选中值的节点列表

### 类型:

    @{
        var valueToMatch = 31;
        //Get the first node inside the root
        var firstTypedContentAtRoot = Umbraco.TypedContentAtRoot().FirstOrDefault();
        if (firstTypedContentAtRoot != null)
        {
            var articles = firstTypedContentAtRoot.Children.Where(x => x.HasValue("character") && x.GetPropertyValue<int>("character") == valueToMatch);
            if (articles.Any())
            {
                <p>Pages with @valueToMatch selected:</p>
                <ul>
                    @foreach (var page in articles)
                    {
                        <li><a href="@page.Url"> @page.Name</a></li>
                    }
                </ul>
            }
        }
    }

### 动态:                             

    @{
		var valueToMatch = 31;
        //Get the first node inside the root
        var firstContentAtRoot = Umbraco.ContentAtRoot().FirstOrDefault();
        if (firstContentAtRoot != null)
        {
            var articles = firstContentAtRoot.Children.Where("character == @0", valueToMatch);
            if (articles.Any())
            {
                <p>Pages with @valueToMatch selected:</p>
                <ul>
                    @foreach (var page in articles)
                    {
                        <li><a href="@page.Url"> @page.Name</a></li>
                    }                      
                </ul>
            }
        }
    }

