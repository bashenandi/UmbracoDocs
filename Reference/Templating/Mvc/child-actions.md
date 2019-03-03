# 在 Umbraco 中使用MVC子Action #

_这部分会说明在 Umbraco 中输出页面时如何使用 MVC 子方法_

## 什么是 MVC 子Action？ ##

ASP.NET MVC中的子Action与ASP .NET Web Forms中的用户控件类似。它允许控制器为了视图的局部输出而执行，就像在 Web Forms 中 你可以执行 UserControler 用于局部输出。

这有一些网上的 MVC 子 Actons 文档：[http://stackoverflow.com/questions/8886433/asp-net-mvc-child-actions](http://stackoverflow.com/questions/8886433/asp-net-mvc-child-actions)

子 Actions 可以非常强大，尤其是当你想要可重用的控制器代码来执行那些你不想在你的视图中执行的代码。这也使得单元测试更容易，因为您只需要测试控制器代码，而不是视图中的代码。

## 创建子Action
这个文档会教你使用[SurfaceControllers](surface-controllers.md)来创建子 Action，但是如果你想要创建自定义的控制器和自己自定义的路由，也是可以的。一旦你创建了`SurfaceController`，你只要创建一个 Action（注意ChildActionOnly属性，这保证了这个方法不是一个通过 URl 的公开路由）：

```csharp
public class MySearchController : SurfaceController
{
    [ChildActionOnly]
    public ActionResult SearchResults(QueryParameters query)
    {
        SearchResults result;
        // TODO: do some searching (perhaps using Examine)
        // using the information contained in the custom class QueryParameters

        // return the SearchResults to the view
        return PartialView("SearchResults", result);
    }
}
```

*注意：在这个示例中我们使用了SurfaceController来创建ChildAction，并且你可以在输出 Umbraco 视图的上下文中使用这个子 Action，然后你就可以使用所有SurfaceController属性，例如UmbracoHelper，UmbracoContext等...*

### Action名称冲突
MVC 允许你的控制器中有同名的重载 Action，然而在有些时候当提交数据并且输出子 Action 到响应中时，可能会遇到一些问题。如果你同时使用了名为`[HttpPost]`的action 和`[ChildActionOnly]` action时，你需要把你的 `[HttpPost]`方法再增加属性`[NotChildAction]`，这样 MVC 就不会混淆了。

## 视图位置
相同的视图位置应用于从子Action返回的部分视图，如此处列出的视图位置：[Partial Views](partial-views.md)

还要注意的是这个示例中使用了Surface控制器，如果我们是将这个控制器作为包的一部分，那么在`~/App_Plugins`的视图也是可以工作的。具体请浏览[SurfaceControllers](surface-controllers.md) 文档*基于插件的控制器*标题下面的内容。

## 输出子Action
在你的视图中输出子 Action 是非常简单的，调用Html.Action方法，通过输入 Action 名和你的控制器名，以及包含路由值的模型。在这里例子中，我们通过一个新的自定义QueryParameters类的实例，并且使用了从 Http 请求中获取的自定义'search'查询字符串：

```csharp
    @Html.Action("SearchResults", "MySearch",
        new { query = new QueryParameters(Request.QueryString["search"]) })
```

*注意：注意我们创建了一个匿名对象，包含属性为'query'，那事因为我们的子 Action 方法接受一个名为'query'的参数，为了正常运行它们必须保持一致。*

这种语法在基于插件的Surface控制器中包含的子Action略有不同。原因在于基于插件的 Surface 控制器会路由到 MVA 的 Area，因此我们需要添加'area'路由参数给这个语句。例如，假设我们有相同的类，但是它标记为基于插件的 Surface 控制器：

```csharp
[SurfaceController("MyCustomSearchPackage")]
public class MySearchController : SurfaceController
{
    [ChildActionOnly]
    public ActionResult SearchResults(QueryParameters query)
    {
        SearchResults result;
        // TODO: do some searching (perhaps using Examine)
        // using the information contained in the custom class QueryParameters

        // return the SearchResults to the view
        return PartialView("SearchResults", result);
    }
}
```

现在输出子 Action 的语法变为：

```csharp
@Html.Action("SearchResults", "MySearch",
    new {
        area = "MyCustomSearchPackage",
    query = new QueryParameters(Request.QueryString["search"])
})
```

唯一的改变是我们告诉它路由到叫做'MyCustomSearchPackage'的'area'。如果这个语法你看着很奇怪，请注意这个路由逻辑和语法是ASP.NET中的标准和非常常见的实践用法。

更多关于子Action的文档以及如何呈现它们，可以在网上找到，也可以在这里找到一个很好的说明：[http://haacked.com/archive/2009/11/18/aspnetmvc2-render-action.aspx](http://haacked.com/archive/2009/11/18/aspnetmvc2-render-action.aspx)