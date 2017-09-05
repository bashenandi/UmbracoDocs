# 多节点树选取器

`别名: Umbraco.MultiNodeTreePicker2`

`返回值: IEnumerable<IPublishedContent>`

## 设置

多节点树选取器允许你配置呈现的树的类别，树中的哪部分是可以被显示的，对于内容来说，允许你使用多节点树选取器选择基于当前文档作为根节点的动态内容。

**节点类型：** 设置节点类型，树的根节点，或者从根节点的查询

对于从根节点的查询蜡树哦，你可以在 xpath 中使用动态占位符，如下面的查询例子：

	//从当前文档之下选择第一个 textpage
	$current/textpage: current page or closest found ancestor
	
	//在父级以下获取类型为 newsArticle的后代
	$parent//newsArticle: parent page or closest found ancestor
	
	//前往内容树的根节点
	$root
	
	//前往 @level=1 的祖先，通常是你的的网站根目录
	$site: Ancestor node at level 1 

很重要的一点是，所有的占位符仅适用于已经发布的内容。因此如果你从当前文档的`$parent`开始获取，Umbraco 会返回它以及距离它最近的已发布后代。在这个例子中，如果父级未发布，它会尝试查询父级的父级，诸如此类。

**根据类别过滤条目:** 允许或不允许树节点包含的文档类别别名。

输入`typeAlias,altTypeAlias`仅允许选择包含他们的节点。输入`!typeAlias,altTypeAlias`，则仅允许选择**不**包含他们的节点。

**最小/最多条目:** 设置允许的最小和最大的条目数量。
 
**注意:** 允许内容树根节点的查询，从7.0.3版本才添加进来 


## 数据类型定义示例

![Multinode Treepicker Data Type Definition](images/Multinode-Treepicker2-DataType.png)

## 内容示例 

![Multinode Treepicker](images/Multinode-Treepicker2-Content.jpg)

## MVC 视图示例

### 类型:

	@{
		var typedMultiNodeTreePicker = Model.Content.GetPropertyValue<IEnumerable<IPublishedContent>>("featuredArticles");
		foreach (var item in typedMultiNodeTreePicker){
			<p>@item.Name</p>
		}
	}