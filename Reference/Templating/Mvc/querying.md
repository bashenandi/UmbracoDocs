# 查询和遍历 #
_这个部分会描述如何在你的 MVC 视图中输出不是当前页面的其他节点的内容_

## 通过id 查询内容和媒体 ##
通过 Id 查询内容的简单使用方法，如下面的语句（假设1234是你想要查询的内容的 id）：
	
	//返回强类型(Umbraco.Core.Models.IPublishedContent)的对象：
	@Umbraco.TypedContent(1234)

	//返回动态表述:	
	@Umbraco.Content(1234) 	

你还可以使用多个 id 查询多个内容：

	//返回强类型(IEnumerable<Umbraco.Core.Models.IPublishedContent>)的集合
	@Umbraco.TypedContent(1234, 4321, 1111, 2222)

	//返回动态表述:	
	@Umbraco.Content(1234, 4321, 1111, 2222)

这个语句支持通过方法查询不限数量的 id。

同样的查询结构也适用于媒体：

	@Umbraco.TypedMedia(9999)
	@Umbraco.TypedMedia(9999,8888,7777)
	@Umbraco.Media(9999)
	@Umbraco.Media(9999,8888,7777)	


## 遍历 ##
所有这些扩展方法都是`Umbraco.Core.Models.IPublishedContent`可用的，因此你可以同时在内容和媒体的强类型对象通过智能提示来操作它们。另外，所有这些方法都试用于动态模型表述。下面的方法都返回`IEnumerable<IPublishedContent>`（如果使用@CurrentPage时返回动态示例）

	Children() //这个使用内容条目的Children 属性是一样的。
	Ancestors()
	Ancestors(int level)
	Ancestors(string nodeTypeAlias)
	AncestorsOrSelf()
	AncestorsOrSelf(int level)
	AncestorsOrSelf(string nodeTypeAlias)
	Descendants()
	Descendants(int level)
	Descendants(string nodeTypeAlias)
	DescendantsOrSelf()
	DescendantsOrSelf(int level)
	DescendantsOrSelf(string nodeTypeAlias)
	
另外还有其他一些方法会返回单个`IPublishedContent`（如果使用@CurrentPage时返回动态示例）

	AncestorOrSelf()
	AncestorOrSelf(int level)
	AncestorOrSelf(string nodeTypeAlias)
	AncestorOrSelf(Func<IPublishedContent, bool> func)
	Up()
	Up(int number)
	Up(string nodeTypeAlias)
	Down()
	Down(int number)
	Down(string nodeTypeAlias)
	Next()
	Next(int number)
	Next(string nodeTypeAlias)
	Previous()
	Previous(int number)
	Previous(string nodeTypeAlias)
	Sibling(int number)
	Sibling(string nodeTypeAlias)

## 复杂查询 (条件) ##
对于`IPublishedContent`模型我们支持开箱即用的强类型 Linq 查询，因此你也可以使用智能提示。我们还支持所有的动态查询操作 Razor 宏，然而在极少数情况下同样的语句不被支持。在某些情况下，动态查询可能较少，而在某些情况下，强类型的输入可能较少，所以最终您将根据你的需要来使用，并且您可以将两者混合。

### 一些示例 ###

#### 子内容是可见的 ####

	//动态操作
	@CurrentPage.Children.Where("Visible")
	
	//强类型操作
	@Model.Content.Children.Where(x => x.IsVisible())

#### 遍历网站地图 ####

	//动态操作
	var values = new Dictionary<string,object>();
	values.Add("maxLevelForSitemap", 4);
	var items = @CurrentPage.Children.Where("Visible && Level <= maxLevelForSitemap", values);
	
	//强类型操作
	var items = @Model.Content.Children.Where(x => x.IsVisible() && x.Level <= 4)

#### 内容子菜单 ####

	//动态操作
	//注意: 你还可以使用NodeTypeAlias ，但是推荐使用DocumentTypeAlias 
	@CurrentPage.AncestorOrSelf(1).Children.Where("DocumentTypeAlias == \"DatatypesFolder\"").First().Children
	
	//强类型
	@Model.Content.AncestorOrSelf(1).Children.Where(x => x.DocumentTypeAlias == "DatatypesFolder").First().Children

#### 复杂查询 ####

有些复杂查询不能写为动态语句，因为动态查询分析器可能无法精确理解你的代码。这就是为什么强类型查询更好的一个原因。

	// This example gets the top level ancestor for the current node, and then gets 
	// the first node found that contains "1173" in the array of comma delimited 
	// values found in a property called 'selectedNodes'.
	// NOTE: This is one of the edge cases where this doesn't work with dynamic execution but the 
	// syntax has been listed here to show you that its much easier to use the strongly typed query 
	// instead

	// dynamic access
	var paramVals = new Dictionary<string, object> {{"splitTerm", new char[] {','}}, {"searchId", "1173"}};
	var result = @CurrentPage.Ancestors().OrderBy("level")
		.Single()
		.Descendants()
		.Where("selectedNodes != null && selectedNodes != String.Empty && selectedNodes.Split(splitTerm).Contains(searchId)", paramVals)
		.FirstOrDefault();
	
	// strongly typed
	var result = @Model.Content.Ancestors().OrderBy(x => x.Level)
		.Single()
		.Descendants()
		.FirstOrDefault(x => x.GetPropertyValue("selectedNodes", "").Split(',').Contains("1173"));
