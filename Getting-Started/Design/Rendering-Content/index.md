# 输出内容 #
_Umbraco中所有模板的主要任务都是用来输出当前页面的值或者查询内容缓存的结果。_

## 输出值 ##
输出当前页面的值有两种不同的方式，要么使用 Umbraco 的 html helper 方法，它允许您通过别名访问每个字段，例如：

	<h1>Hello @Umbraco.Field("pageName")</h1>
	<p>@Umbraco.Field("bodyText")</p>

后台的模板编辑器中有一个对话框（点击![按钮](images/button.png)），它可以帮助你选择值进行修改：

![对话框](images/dialog.png)

## 使用 Model 输出字段 ##
UmbracoHelper提供了很多有用的参数来决定如何输出值。如果你想简单的输出值，你可以使用视图的`@Model.Content`属性。一个示例：

	@Model.Content.GetPropertyValue("bodyContent")

你还可以指定你想从属性中输出的类型。如果属性编辑器或者值不支持这个类型的转换，会抛出一个异常。一个示例：

	@Model.Content.GetPropertyValue<double>("amount")

## 查询内容 ##
在许多例子中，你想要的不止是从当前页面输出值，例如在导航中创建页面列表 - 或者生成 sitemap。你可以在你的模板中通过查询内容来实现：

	<ul>
		@foreach(var child in Model.Content.Children()){
			<li><a href="@child.Url">@child.Name</a></li>
		}
	</ul>

你可以在模板编辑器中使用查询帮助（![Query button](images/query-button.png)）来组建更加复杂的查询：

![Query helper](images/query.png)

###更多内容
- [Razor示例](../../../Reference/Templating/Mvc/examples.md)
- [查询](../../..//Reference/Templating/Mvc/querying.md)

### Umbraco TV
- [Episode: Setting up our first template](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/templating/alt-template/)
- [Episode: Insert Umbraco page field dialog](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/templating/insert-umbraco-page-field-dialog/)