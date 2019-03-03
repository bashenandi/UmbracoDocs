# 使用主模板创建更多页面 - 第二部分 #

## 创建联系我们页面 
现在我们制作一个简单的页面，用来把我们的联系信息放进去。要获得更多的功能，您可能需要将其替换为一个完全成熟的“联系我们”表单。

一些潜在的解决方案：

* 使用Surface Controllers构建你的联系表单 - [/Reference/Templating/Mvc/forms](http://our.umbraco.org/documentation/Reference/Templating/Mvc/forms) 或者 [http://umbraco.tv/videos/developer/fundamentals/surface-controllers/](http://our.umbraco.org/documentation/Reference/Templating/Mvc/forms)
* 如果你不是程序员你可以使用 Umbraco 内置的软件包 - Umbraco Forms。这还有个额外的好处，是编辑者也可以创建它们自己的表单。 [http://umbraco.com/products-and-support/forms](http://umbraco.com/products-and-support/forms)

### 创建一个只有简单内容的联系我们页面 ###

For now, let's create a simple content-only contact us page - a page where the user can provide a title and some rich text. This is very similar to our homepage document type at the moment but we're assuming that you'll go and develop this into something very specific (e.g. adding the featured article and other article content blocks).

现在让我们创建一个简单的只有内容的联系我们页面 - 这个页面可以给用户提供标题和一些富文本。这与我们homepage文档类型非常相似，但我们假设您将把它开发成非常具体的东西（例如添加精选文章和其他文章内容块）。

打开**_Settings > Document Types_**（鼠标移入）**_> ... > + Create_ > Document Type**。我们创建一个名为"_Simple Content Page_"的文档类型。

首先选择一个**_Icon_** - 输入单词"_Content_"到过滤文本框，选择文档图标。在描述中，输入"A simple content page"。点击**_Save_**。

现在点击**_Settings > Templates > ..._**，然后选择**_Reload Nodes_** 来显示刚才创建文档类型时自动创建的Simple Content Page模板。点击**_Simple Content Page 节点_** 之后打开**_Properties_**标签页，在**_Master template_**下拉列表中选择"_Master_" - 这意味着将从主模板中获取头部和底部，就像Homepage 模板一样。

点击**_Save_**后载入**_Template_ 标签页**，你会看到 Razor 代码已经更新为`Layout ="Master.cshtml"`，如果没它没有自己更新，点击其他节点再返回来重新加载。把下面的 HTML 代码贴进模板中，然后点击**_Save_**。

	<div id="main-container">
		<div id="main" class="wrapper clearfix">
			<section>
				<h2>Header goes here</h2>
				<p>Content goes here</p>
			</section>	
		</div> <!-- #main -->
	</div> <!-- #main-container -->

*Figure 30 - Simple Content Page 模板 HTML*

现在使用新的**_文档类型_**和**_模板_**创建一个新的页面 - 前往**_Content > Homepage (鼠标移入) > ... > Create_**。然而我们并不能做什么！我们看到了下面的错误：

![Umbraco 内容错误 - 没有可用文档类型](images/figure-31-simple-content-page-cant-create.png)

*Figure 31 - Umbraco 内容错误 - 没有可用文档类型*

这是有意而为的 - Umbraco仅允许编辑者在你，开发者，允许的地方创建内容。这将阻止用户在后面创建的新闻容器节点下创建新的首页，从而破坏你的站点设计（或者整个网站）！不幸的是这个功能会让很多Umbraco 新手感到困惑 - 因此我们在这里为你展示这个错误。

前往**_Settings > Document Types > Homepage_**，打开**_Permissions_**窗口，你会看到**_Allowed child node types_**（注意不要和**_Templates_**窗口的**_Allowed templates_**混淆 - 我们会在后面讲到）列表。我们要允许用户能够在 Homepage 下面创建类型为"_Simple Content Page_"的子节点。点击**_Add child_**选择**_Simple Content Page_**后点击**_Save_**。

![Homepage - 允许的子节点类型](images/figure-32-homepage-allowed-child.png)

*Figure 32 - Homepage - 允许的子节点类型*

这正是让人困惑的地方 - 首先我们创建了**_Simple Content Page_**类型，但是之后却必须允许它可以创建在 Homepage 文档类型的下面 - 例如我们创建了新的**_Document Type_**，但是接下来必须更新**_Homepage_**设置，来允许使用它。当我们创建文章容器和文章条目时要再次做这些 - 我们需要允许文章条目创建在容器下面。简单 - 也许不是，但你会习惯的！

现在返回 **Content > Homepage (鼠标移入) > ... > Create**，现在我们有了**Simple Content Page**选项！选择它然后输入名字（顶部的输入框） - 输入名字为"Contact Us"。你可以看到在这里只有**_Properties_**标签页 - 没有任何数据属性。这不同于 Homepage 文档类型，我们没有添加任何标签页和数据属性（例如没有bodyText 或者 pageTitle字段来输入内容！）。点击**_Save and Publish_**。

![创建联系我们页面](images/figure-33-contact-us.png)

*Figure 33 - 创建联系我们页面*

我们的**_Content树_**现在应该会重新加载，会有一个**_Contact Us_**位于 Homepage 的下级。对于多数网站来说这是建议的结构 - 主要的一级页面应该在 Homepage 之下。看一下这些页面 - 你在**_Properties 标签页_**，会看到**_Link To document row_**  - 点击它。你会再次看到一个无样式的页面。这是因为模板设计者还是假定你的站点应该是扁平的结构 - 例如所有的页面都在同一等级，所以当页面等级低于 homepage 时，浏览器无法找到 CSS 和 JavaScript。你需要更新**_Master_**模板，给 JS 和 CSS 源代码所在位置添加斜杠。

例如在主模板中改变下面的信息：

### CSS ###

由: `<link rel="stylesheet" href="css/style.css">`

改为: `<link rel="stylesheet" href="/css/style.css">`

### JavaScript ###

由: `<script src="js/libs/modernizr-2.0.6.min.js"></script>`

改为：`<script src="/js/libs/modernizr-2.0.6.min.js"></script>`

**_Save_** 模板并重新加载你的**_Contact Us page_**。我们现在会看到友好的空白页面。

让我们添加两个简单的字段 - **_pageTitle_** (type = Textstring) 和 **_bodyText_** (type Rich Text Editor)。如果你不确定该怎么做，跟随创建 Homepage 文档类型的操作。然后连接这些字段 - 通过编辑模板，如果你再次忘记，还是跟着 Homepage 部分的操作进行！

你会看到截图，我们删除了其中的`<p>`标签 - 富文本编辑器会为我们添加这些(文本字段还是需要`<h2>`标签)。

![简单内容页面常见属性](images/figure-34-contact-us-generic-properties.png)
*Figure 34 - 简单内容页面常见属性*


![带有数据字段的简单内容页面](images/figure-35-contact-us-template-with-data-fields.png)
*Figure 35 - 带有数据字段的简单内容页面*

现在添加一些内容到**_Content > Homepage 节点 > Contact Us 节点_**中。点击**_Save and Publish_**后当你再次加载时，会看到一个稍微有意思点的页面了！

![带有数据的联系我们](images/figure-36-contact-us-with-some-data.png)
*Figure 36 - 带有数据的联系我们*

## 使用 Homepage 中的文档类型属性 ##

你可能注意到了现在底部是空的 - 我们没有从 Homepage 节点获取到内容。我们需要告诉 Umbraco 从父级的**_Homepage_**中获取内容。因此我们要编辑这个模板（Master模板）。

选中底部`<h3>`标签的 Umbraco 字段，然后再次点击**_Insert Umbraco page field_**按钮![Umbraco Page Field Button](images/umbraco-page-field.png)，这是我们早些时候忽略的所有选项开始发挥作用的地方 - 再次从**_Choose field_**下拉列表中选择footerText，但是这一次我们要勾选**_Recursive_**复选框。这是告诉 Umbraco，如果我们请求的节点（例如联系我们）不存在这个字段，它会在内容树中向上查找（因此在这个示例中会前往**_Homepage_**获取这个内容）- 这也意味着如果你想让编辑者也可以复写这个网站默认信息，你可以给联系我们页面添加**_footerText_**元素，但这并不常用。点击**_Insert_**，你会看到 Razor 有一点不同：`@Umbraco.Field("footerText", recursive: true)` 

点击**_Save_**再重新加载联系我们页面。

## 下一步 - [主模板 - 导航菜单](Master-Template-The-Navigation-Menu.md) ##
给模板添加菜单的简单方案。