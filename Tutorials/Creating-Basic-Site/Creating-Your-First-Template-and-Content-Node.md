# 创建（编辑）第一个模板
接下来在**_设置_**菜单后面的**_Templates_**文件夹上，点击展开节点图标（是一个小的三角）- 你会看到一个名为"_Homepage_"的子节点 - Umbraco 会在我们创建**_文档类型_**是为我们自动创建（记得还有一个选项叫"..without template", 我们选择了这个，而这就是它做的）。

点击 **_Homepage_** 节点会载入这个模板 - 可以看到除了少量的 Razor 代码，它是空白的！

![空白的 Homepage 模板](images/figure-13-empty-homepage-template.png)

*图 13 - Homepage 模板*

保持这些代码（如果你不明白它们的意思，先不要担心！），让我们再拷贝一些模板代码进来。从**_初始_**模板中，打开**_index.html_**文件到你喜欢的文本编辑器（Notepad++是不错的选择）。复制粘贴整体代码到这个模板文件的关闭括号"}"**之后**。你的模板现在看起来应该是下面这样：

![Homepage Template with Initializr HTML](images/figure-14-homepage-template-with-initializr-html.png)

*图 14 – 包含初始 HTML 的 Homepage 模板*

点击**_保存_**按钮。

现在我们有了一个模板。这是我们完成第一个网页所要做的三个阶段中的第个。

# **创建第一个内容节点**

第三和最后一个阶段，我们要在 Umbraco 中创建第一个页面，也就是创建一个内容节点用于添加内容供 Umbraco 使用，连同文档类型和模板，将 HTML 页面提供给网页浏览者。

现在我们准备创建在 Umbraco 中的第一个页面！点击**_Content_**按钮（左侧菜单的第一个选项）。

鼠标移动到灰色的文字**_CONTENT_**上，你会看到三个点**_..._** - 点击。如果所有的事情你都正确的完成了，你会看到一个选项用于创建 Homepage！

![Create a Homepage](images/figure-15-create-a-homepage.png)

*图 15 - 创建Homepage*

>如果你没有看到这个也不要惊慌 - 前去检查 **_Settings> Document Types > Homepage_**  > **_Structure tab_** > **_Allow at root_**是勾选了的。

现在让我们创建首页。点击图标你会看到已有的设置 - 文档类型驱动我们的 homepage 内容 - 它给我们和编辑者提供了所需的字段。

在页面的顶部你会看到一个字段 "_Enter a name..._" ，点击输入新内容节点的名字。我们叫它"_Homepage_"。

![Create a Homepage](images/figure-15a-create-a-homepage-enter-name.png)

*图 15a - 创建首页 - Enter a name...*

在**_Contents_** 标签页中输入下列内容：

>**_Page Title_** 	Welcome to Widgets Ltd
>
>**_Body Text_**    Hello world! We can write what we like here!
>"Widgets Ltd 2014"

点击 **_Footer_** 标签页输入：

>**Footer Text**	 "Copyright Widgets Ltd 2014" 

现在点击绿色的 **_Save and publish_** 按钮。在 **_CONTENT_** 标签下的首页节点菜单会重新加载。前往去浏览器中刷新你的网页http://localhost - 默认的 Umrbaco 也免回打开，看起来光秃秃的没有样式的页面！我们到哪里了！

>如果你看到空白的页面，检查你输入模板中的标记并且记得保存。

![An Unstyled Homepage](images/figure-16-unstyled-homepage.png)

*图 16 – 一个没有样式的首页*

---
## 下一步 - [CSS &amp; Javascript](CSS-And-Javascript.md)
在 Umbraco 中给你的网站添加 CSS 和 Javascript。
