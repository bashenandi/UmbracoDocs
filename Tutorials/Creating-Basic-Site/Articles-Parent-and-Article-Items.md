#文章父级和文章条目 - 一个有无数子页面的父页面
有一个文章的父页面和许多继承的子文章，编辑者能够任意添加页面，Umbraco 会提供很好的例子。我们假设我们虚构的部件有限公司，每月写大约10篇文章，并希望文章页面像博客一样运作(例如，你可以将这个功能用于博客、新闻和事件页面)。

创建两个新的文档类型"_Articles Main_" 和 "_Articles Item_"（**_Document Types Settings > Document Types (hover) > ... > + Create_**）。记得使用带模板的选项，来帮你创建模板。

创建下面的**_标签页_**和**_数据属性_**：
####Articles Main
>Tab = Intro

>**"Articles Title"** - Type = Textbox

>**"Articles Body Text"** - Type = Rich Text Editor**


![Articles Main Document Type Data Properties](images/figure-38-articles-main.png)
*图 38 - Articles Main 文档类型数据属性*


####Articles Item
>Tab = Contents

>**"Article Title"** - Type = Textstring

>**"Article Contents"** - type = Rich Text Editor**


![Article Item Document Type Data Properties](images/figure-39-articles-item.png)
*图 39 - Article Item 文档类型数据属性*

现在去**_Settings > Document Types >Articles Main node > Permissions screen > Allowed child nodetypes_** 添加**_Articles Item_**。这样才能允许我们在文章主页下面创建条目（作为父容器存在）。我们还需要允许**_Articles Main node_**创建在**_Homepage node_**下面（去**_Settings > Document Types > Homepage node > Permissions screen >  Allowed child node types_**设置 - 不要添加**_Articles Item_** - 仅允许它创建在文章主页下面）。

现在去**_Content > Homepage 节点 (鼠标移入)> ..._**创建一个**_Articles Main_**（如果你看不见这个选项，返回检查你的 _allowed child nodes_ 设置 - 是否忘了点击**_保存_**？）类型的节点，叫做"_Articles_"。给这个节点输入一些内容和标题，然后在下面创建几个文章条目内容（**_Content > Homepage node > Articles node (鼠标移入) >  ..._** 创建）。

现在你有了一个看起来如下图所示的内容树（显示你自己页面节点的名字）。现在去更新我们刚刚创建的模板（创建文档类型时自动创建的）。首页更新它们都去使用 Master 作为父级模板 **_Settings > Templates > Articles Main node > Properties tab > Master template dropdown_** = "Master" - 再对Articles Item模板做同样的事情，记得点击**_保存_**。

![Content Tree With Articles](images/figure-40-articles-created.png)
*图 40 - 带文章的内容树*

从**_Simple Content Page_**模板中复制模板内容粘贴到Articles Item 和 Articles Main模板中（你可能需要刷新节点才能看到它们。将主版本都设置为"Master"并且将其中的页面字段标签替换为相关的属性，例如：**_Articles Main_**模板中的**_articlesTitle_** 和 **_articlesBodyText_** 以及 **_Article Item_**模板中的**_articleTitle_** and **_articleContents_**）。


>注意当复制时不要覆盖了第一行`@inherits Umbraco.Web.Mvc.UmbracoTemplatePage<ContentModels.ArticlesMain>` - 如果在加载页面时抛出异常，可能是没有绑定源，确保最后一部分< >尖括号中的内容必须和你的文档类型别名匹配。

如果现在我们去浏览器中访问Articles Main页面，会看到填写的内容。我们希望把子文章列表显示在介绍内容的下面，以便浏览者能够看到文章列表。 Umbraco 可以让这变的很容易，但是我们需要使用一些 Razor 代码来实现。

点击左侧菜单的**_Developer_**菜单，把鼠标移动到**_Partial View Macros Files_**节点上面后点击**_..._**，然后再点击**_Create_**。命名为"_listArticles_"并且在**_Choose a snippet_**中选择"_List Child Pages Ordered By Date_"，最后点击**_Create_**。

现在我们必须要做的连接Articles主页面来显示子文章。编辑Articles Main 模板**_Settings > Templates node > Master node > Articles Main node > Template tab_**，在*articlesBodyText*标签下面按下回车，然后点击**_Insert Macro_**按钮，选择刚才创建的"ListArticles"宏然后点击**_Save_**。

![Template for Articles Parent with the Macro Code](images/figure-41-articles-parent-with-macro-code.png)
*图 41 - 带有宏代码的文章父级模板*

现在访问**_Articles_**查看刚才做的内容 - 我们已经获取到了一些东西！现在我们让他变得更加真实 - 要更好的理解这些需要学习Razor或者 Ummbraco 视频，但是我们只需要让网站更加漂亮一点 - 编辑刚才创建的宏**_Developer > Partial View Macro Files > listArticles.cshtml_**，更改内容如下：

    @inherits Umbraco.Web.Macros.PartialViewMacroPage
    @{ 
	    var selection = CurrentPage.Children.Where("Visible").OrderBy("CreateDate desc"); 
	    @* OrderBy() takes the property to sort by and optionally order desc/asc *@
    }

    @foreach (var item in selection){
    	<div class="article">
    		<div class="articletitle"><a href="@item.Url">@item.Name</a></div>
    		<div class="articlepreview">@Umbraco.Truncate(@item.ArticleContents,100) <a href="@item.Url">Read More..</a></div>
    	</div>
    	<hr/>
    }

*图 42 - 优化listArticles 模板*

现在在浏览器中检查这些！

![Finished Articles Page](images/figure-43-finished-articles-page.png)
*图 43 - 完成文章列表页*


---
##Next - [结论和下一步?](Conclusions-Where-Next.md)
到目前你应该有了一个基础的网站 - 但是下一步呢？你仅仅了解了Umbraco 强大的一些表面功能 - 这里还有一些链接告诉你更多！

