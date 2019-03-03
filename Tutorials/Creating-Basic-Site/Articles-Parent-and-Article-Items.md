# 文章的父级和文章条目 - 一个有无数子页面的父页面 #

Having an Articles Parent page, and a number of associated child articles which the editors can add to freely, provides a good example page of the power of Umbraco. We'll assume our fictional company, Widgets Ltd, write about ten articles a month and want the articles page to act like a blog (e.g. you could use this functionality for a blog or news and events pages).

强大的Umbraco提供了一个很好的示例页面，包含一些文章的父级页面，以及一些继承的子级文章，而编辑人员可以自由地添加它们。我们假设一个虚构的部件有限公司，每月写大约10篇文章，并希望文章页面像博客一样运作(例如，你可以将这个功能用于博客、新闻和事件页面)。

创建两个新的文档类型"_Articles Main_" 和 "_Articles Item_"（**_Document Types Settings > Document Types (hover) > ... > + Create_**）。记得使用带模板的选项，来帮你创建模板。

创建下面的**_标签页_**和**_数据属性_**：

#### Article Main ####
>Tab = Intro

>**"Articles Title"** - 类型 = Textbox

>**"Articles Body Text"** - 类型 = Rich Text Editor**


![文章主页文档类型数据属性](images/figure-38-articles-main.png)

*Figure 38 - 文章主页文档类型数据属性*


#### Article Item ####
>Tab = Contents

>**"Article Title"** - 类型 = Textstring

>**"Article Contents"** - 类型 = Rich Text Editor**


![文章条目文档类型数据属性](images/figure-39-articles-item.png)

*Figure 39 - 文章条目文档类型数据属性*

前去**_Settings > Document Types >Articles Main 节点 > Permissions 窗口 > Allowed child nodetypes_** 添加**_Articles Item_**。这样才能允许我们在文章主页下面创建条目（作为父容器存在）。我们还需要允许**_Articles Main 节点_**可以创建在**_Homepage 节点_**下面（前去**_Settings > Document Types > Homepage 节点 > Permissions 窗口 >  Allowed child node types_**设置 - 不要添加**_Articles Item_** - 我们仅允许它创建在文章主页的下级）。

接下来我们就可以去**_Content > Homepage 节点 (鼠标移入)> ..._**创建一个名为"Articles"的**_Articles Main_**（如果你看不见这个选项，返回检查你的 _allowed child nodes_ 设置 - 是否忘了点击**_保存_**？）类型的节点，叫做"_Articles_"。给这个节点输入一些内容和一个标题，然后在这个节点的下级创建几个文章条目内容（**_Content > Homepage 节点 > Articles 节点 (鼠标移入) >  ..._** 创建）。

现在你有了一个看起来如下图所示的内容树（显示出的是自己页面节点的名字）。现在去更新我们刚刚创建的模板（创建文档类型时自动创建的）。首先让它们使用 Master 作为父级模板 **_Settings > Templates > Articles Main 节点 > Properties 标签页 > Master template dropdown_** = "Master" - 再对Articles Item模板做同样的事情，记得点击**_保存_**。


![带一些文章的内容树](images/figure-40-articles-created.png)
*图 40 - 带一些文章的内容树*

从**_Simple Content Page_**模板中复制内容粘贴到Articles Item 和 Articles Main模板中（你可能需要刷新节点才能看到它们。将主版本都设置为"Master"并且将其中的页面字段标签替换为相关的属性，例如：**_Articles Main_**模板中的**_articlesTitle_** 和 **_articlesBodyText_** 以及 **_Article Item_**模板中的**_articleTitle_** and **_articleContents_**）。

>注意在粘贴的时候不要覆盖了第一行`@inherits Umbraco.Web.Mvc.UmbracoTemplatePage<ContentModels.ArticlesMain>` - 如果在加载页面时抛出异常，可能是没有绑定源，确保最后一部分< >尖括号中的内容必须和你的文档类型别名一致。

如果现在我们去浏览器中查看Articles Main页面，就会看到我们想要看到的内容了。我们希望把子级文章列表显示在介绍内容的下面，以便浏览者能够看到更多的文章列表。 Umbraco 让这变的很容易，但是我们需要使用一些 Razor 代码来实现。

点击左侧菜单的**_Developer_**菜单，把鼠标移动到**_Partial View Macros Files节点_**上面点击**_..._**，然后再**点击_+Create_**。输入名字"_listArticles_"，并在**_Choose a snippet_**中选择"_List Child Pages Ordered By Date_"，最后点击**_Create_**。

现在我们要做的是把文章主页链接起来，列表显示我们的子级文章。编辑文章主页模板**_Settings > Templates 节点 > Master 节点 > Articles Main 节点 > Template 标签页_**。在*articlesBodyText*标签下方输入回车，并且点击**_Insert Macro_** 按钮，选择我们刚才创建的ListArticles宏，然后点击**_Save_**。

![带有宏的文章父级模板](images/figure-41-articles-parent-with-macro-code.png)
*Figure 41 - 带有宏的文章父级模板*

访问我们刚才完成**_Articles_**页面 - 我们已经获取到了一些东西！现在我们让他变得更加实用 - 要更好的理解Razor需要学习一些课程或者 Ummbraco 视频，而我们目前只是要让网站更加漂亮一点 - 编辑刚才创建的宏**_Developer > Partial View Macro Files > listArticles.cshtml_**，更改内容如下：

    @inherits Umbraco.Web.Macros.PartialViewMacroPage
    @{ 
	    var selection = CurrentPage.Children.Where("Visible").OrderBy("CreateDate desc"); 
	    @* OrderBy() 是一个排序的属性方法，可选项是desc/asc  *@
    }

    @foreach (var item in selection){
    	<div class="article">
    		<div class="articletitle"><a href="@item.Url">@item.Name</a></div>
    		<div class="articlepreview">@Umbraco.Truncate(@item.ArticleContents,100) <a href="@item.Url">Read More..</a></div>
    	</div>
    	<hr/>
    }

*Figure 42 - 改进listArticles 使用的宏*

现在到浏览器中检查这个页面！

![完成的文章列表页面](images/figure-43-finished-articles-page.png)
*Figure 43 - 完成的文章列表页面*


## [结论和下一步?](Conclusions-Where-Next.md) ##
到目前为止，你应该有了一个基本的运行网站 - 但是下一步呢？你还没有触及到 Umbraco 更加强大的一面 - 这里有一些链接可以让你继续深入！
