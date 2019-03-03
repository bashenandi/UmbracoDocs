# 创建（编辑）第一个模板 #


<<<<<<< HEAD

点击**Settings > Templates**文件夹左边的节点展开图标（是一个小的三角形） - 你会看到一个标题为"Homepage"的子节点 - 在我们创建**文档类型**时Umbraco 自动创建了它（记得还有一个选项叫"..without template"，我们选择了有模板的，而这就是它起到的作用）。

点击 **_Homepage_** 节点来加载这个模板 - 可以看到除了少量的 Razor 代码，它是空白的！

![空白的 Homepage 模板](images/figure-13-empty-homepage-template.png)

*图 13 - Homepage 模板*

先不要管这些代码（如果你不明白它们的意思，先不要担心！），我们把我们的模板代码复制进来。用你喜欢的文本编辑器（Notepad++是不错的选择），从**_初始_**模板包中打开**_index.html_**文件。复制粘贴整体代码到这个模板文件的关闭括号"}"**之后**。你的模板现在看起来应该是下面这样：



![包含初始 HTML 的 Homepage 模板](images/figure-14-homepage-template-with-initializr-html.png)

*图 14 – 包含初始 HTML 的 Homepage 模板*

点击**_保存_**按钮。

现在我们有了一个模板。这是我们第一个网页所要完成三个阶段中的第二个。

# **创建第一个内容节点** #

第三也是最后一个阶段，我们要在 Umbraco 中创建第一个页面，也就是创建一个内容节点用于添加内容供 Umbraco 使用，连同文档类型和模板，将 HTML 页面提供给网页浏览者。

我们现在准备在 Umbraco 中创建我们的第一个页面！点击**_Content_**按钮（左侧菜单的第一个选项）。

鼠标移动到灰色的文字**_CONTENT_**上，你会看到三个点**_..._** - 点击。如果所有的事情你都正确的完成了，你会看到一个选项用来创建 Homepage！

![创建Homepage*](images/figure-15-create-a-homepage.png)

*图 15 - 创建Homepage*

>如果你没有看到也不要惊慌 - 前去 **_Settings> Document Types > Homepage_**  > **_Structure tab_** > **_Allow at root_**检查是否已经勾选。

现在创建我们的首页。点击图标你会看到我们做好的设置 - 文档类型驱动我们的 homepage 内容 - 它给我们和编辑者提供了它们所需的字段。

在页面的顶部你会看到一个字段 "_Enter a name..._" ，点击输入新内容节点的名字。我们叫它"_Homepage_"。

![创建首页 - Enter a name...](images/figure-15a-create-a-homepage-enter-name.png)

*图 15a - 创建首页 - Enter a name...*

在**_Contents_** 标签页中填充下面的内容内容：

>**_Page Title_** 	Welcome to Widgets Ltd
>
>**_Body Text_**    Hello world! We can write what we like here!
>"Widgets Ltd 2014"

点击 **_Footer_** 标签页输入：

>**Footer Text**	 "Copyright Widgets Ltd 2014" 

现在点击绿色的 **_Save and publish_** 按钮。在 **_CONTENT_** 标签下的首页节点菜单会重新加载。前往去浏览器中刷新你的网页http://localhost - 默认的 Umrbaco 页面打开，看起来光秃秃的没有样式的页面！我们到哪里了！

>如果你看到空白的页面，检查你输入模板中的标记并且记得保存。

![一个没有样式的首页](images/figure-16-unstyled-homepage.png)

*图 16 – 一个没有样式的首页*

## Next - [CSS & JavaScript](CSS-And-JavaScript.md)
在 Umbraco 中给你的网站添加 CSS 和 JavaScript。