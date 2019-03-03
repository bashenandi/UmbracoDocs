<<<<<<< HEAD
# 下拉列表, 公开键值
=======
# (Obsolete) Dropdown List, Publishing Keys
>>>>>>> upstream/master

`返回值: Int`

根据预设值返回列表。保存值为选中值的 id。

## 设置

<<<<<<< HEAD
### 预设值
你可以添加、编辑和删除预设值的数据，以显示在下拉列表中。
=======
### Prevalues

You can add, edit & delete the data prevalues rendered within the dropdown list.
>>>>>>> upstream/master

## Data Type Definition Example

![Dropdown List Data Type Definition](images/Dropdown-List-Keys-DataType.png)

## Content Example

![Dropdown List Content](images/Dropdown-List-Keys-Content.png)

## MVC 视图示例输出选中值

<<<<<<< HEAD
### 类型:
=======
### Simple typed example
>>>>>>> upstream/master

```csharp
@if (Model.Content.HasValue("character"))
{
    var preValue = Umbraco.GetPreValueAsString(Model.Content.GetPropertyValue<int>("character"));
    <p>@preValue</p>
}
```

<<<<<<< HEAD
### 动态:     
                         
    @if (CurrentPage.HasValue("character"))
    {
        var preValue = Umbraco.GetPreValueAsString(CurrentPage.character);
        <p>@preValue</p>        
    }    
=======
### Simple dynamic example

:::warning
See [Common pitfalls](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics) for more information about why the dynamic approach is obsolete.
:::

```csharp
@if (CurrentPage.HasValue("character"))
{
    var preValue = Umbraco.GetPreValueAsString(CurrentPage.character);
    <p>@preValue</p>
}
```
>>>>>>> upstream/master

## MVC 视图示例列出所有包含下拉列表选中值的节点列表

<<<<<<< HEAD
### 类型:
=======
### Typed
>>>>>>> upstream/master

```csharp
@{
    var valueToMatch = 31;
    // Get the first node inside the root
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
```

### Dynamic

<<<<<<< HEAD
### 动态:                             
=======
:::warning
See [Common pitfalls](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics) for more information about why the dynamic approach is obsolete.
:::
>>>>>>> upstream/master

```csharp
@{
    var valueToMatch = 31;
    // Get the first node inside the root
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
```
