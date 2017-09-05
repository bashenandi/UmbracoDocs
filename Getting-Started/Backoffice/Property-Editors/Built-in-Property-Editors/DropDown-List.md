# 下拉列表

`返回值: String`

根据预设值显示列表。这个值保存的是文本值。

## 设置

###预设值
你可以添加、编辑和删除预设值的数据，以显示在下拉列表中。

##数据类型定义示例

![Dropdown List Data Type Definition](images/Dropdown-List-DataType.png)

##内容示例 

![Downdown List Content](images/Dropdown-List-Content.png)

##MVC 视图示例输出已选的值

###类型:

    @if (Model.Content.HasValue("superHero"))
    {
        <p>@Model.Content.GetPropertyValue("superHero")</p>
    }

###动态:     
                         
    @if (CurrentPage.HasValue("superHero"))
    {
        <p>@CurrentPage.superHero</p>
    }    

##MVC 视图示例列出所有包含下拉列表选中值的节点列表

###类型:

    @{
        var valueToMatch = "SuperMan";
        //Get the first node inside the root
        var firstTypedContentAtRoot = Umbraco.TypedContentAtRoot().FirstOrDefault();
        if (firstTypedContentAtRoot != null)
        {
            var articles = firstTypedContentAtRoot.Children.Where(x => x.HasValue("superHero") && x.GetPropertyValue<string>("superHero").ToLower().Contains(valueToMatch.ToLower()));
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

###动态:                             

    @{
		var valueToMatch = "SuperMan";
        //Get the first node inside the root
        var firstContentAtRoot = Umbraco.ContentAtRoot().FirstOrDefault();
        if (firstContentAtRoot != null)
        {
            var articles = firstContentAtRoot.Children.Where("superHero.ToLower() == @0", valueToMatch.ToLower());
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

