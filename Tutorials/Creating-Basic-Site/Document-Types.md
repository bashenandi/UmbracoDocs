# 文档类型 #

## 数据优先 ##

### 没有输入 = 没有输出! ###

任何网站的第一步都是创建一个 "**_Document Type_**" - 在经过几次安装后你会熟悉这个术语，但是在开始可能会有一些困惑。在 Umbraco 中 **_Document Type_** 是一个数据容器，你可以添加数据字段/属性在其中，编辑用户也可以输入这些数据，Umbraco 也可以使用它们并在 "**_template_**" 相关位置输出它们（关于这些稍后会介绍）。

**_Document Types_** 可以无限扩展，但是通常你只需要添加下面的数据字段：

*    Page title
*    Sub Heading
*    Body Text
*    Meta Title
*    Meta Description
*    ...


每个 **_Data Field_** 都是一个类型 - 例如文本字符串、数字、富文本内容……这些会在后面介绍。


## 创建第一个文档类型 ##
现在，让我们开始吧。前往 **_Settings_** 菜单。位于左侧黑色菜单中的第三个带有扳手的按钮。然后你会看到很长的列表 - 不要担心这些，当我们需要它们的时候会介绍到它们。

**_Document Types_** 现在（从v7.4）位于列表中的第一个位置，并且在所有 Umbraco 的构建中都是最开始的点。当鼠标移动到 **_Document Types_** **_节点_** 上面，你会看到三个点 **_..._** ，点击后会看到菜单。你会看到三个选项，点击 **_Document Type_** 选项 - 我们想让模板自动创建。使用文件夹可以帮助你组织你的文档类型，但是现在我们先让这些事情变得简单一些。

![创建一个文档类型](images/figure-7-creating-a-document-type.png)

*图 7 - 创建一个文档类型*


将我们新建的**_Document Type_**起个**_名字_**叫 "_HomePage_"，你会看到别名也已经被创建了。

在**_Description_** 字段中输入 "_This is our homepage template_"。这个字段用来帮助我们在后期选择正确的文档类型。

点击 **_Save_** 存储我们新的文档类型。


![命名新的文档类型](images/figure-8-name-your-document-type.png)

*图 8 – 命名新的文档类型*


Umbraco 现在添加了一个**_Document Type_**到对应节点下面的树结构中。现在我们去给我们的文档类型设置一个图标，帮助我们的编辑者后期在 **_Content_** 中识别它。点击名称字段前面的白色文档图标，在显示的搜索字段中输入"_home_"，将会显示一个房子的图标，选中它作为文档图标。


![给文档类型添加图标](images/figure-9-adding-an-icon-to-document-type.png)

*图 9 - 给文档类型添加图标*


下一步点击 **_Permissions_**图标勾选**_Allow as root_**。这将允许我们在内容树的根节点中创建 homepage（简单吧？）。


![允许在根节点创建 homepage](images/figure-9a-allow-document-type-as-root.png)

*图 9a - 允许在根节点创建 homepage*

接下来我们返回 **_Design_** 界面。创建一个新的标签页，称为"_Contents_"，记得要点击 **_Save_**。


![文档类型 - 添加第一个内容标签页](images/figure-10-document-types-adding-tabs.png)

*图 10 - 文档类型 - 添加第一个内容标签页*

接下来点击**_Add property_**链接 - 在这里我们依次创建必要的数据容器，使编辑者能够输入homepage所需的内容。在**_Name_**中输入"_Page Title_"。当你移动到下个输入框时，你会发现Umbraco 已经帮你生成了别名"pageTitle"。点击 **_Add editor_**链接，你会看到很长的编辑器列表，选择"Textbox" (晚些我们再回来浏览这些数据类型列表 - 这正是使 Umbraco 强大的原因，但是现在我们只使用一些简单的数据类型)。

![选择文本输入框数据类型](images/figure-11a-selecting-textbox-data-type.png)

*图 11.1 - 选择文本输入框数据类型*

Umbraco 会为这个数据类型生成一个很长的名字 - 暂时忽略这一点，选择 **_Submit_**。

我们还可以输入**_Description_**，可以进一步提供相关内容帮助编辑者了解它，在这里我们输入"_The main title of the page (例如 Welcome to Widgets Ltd)._

![创建 PageTitle 数据类型](images/figure-11-creating-our-pagetitle-data-type.png)

*图 11.2 - 创建 PageTitle 数据类型*

现在先忽略其余的字段，点击右下角的绿色**_Submit_**按钮。

重复这个步骤，点击**_Content_**标签页底部的**_Add property_**创建下面的属性（使用**_Add another tab_**链接创建新的标签页，称为 Footer，来放置 Footer Text）：

<table border="0">
<col width="130">
<col width="400">
<tr><th>Name</th><th>Body Text</th></tr>
<tr><td>Alias:</td><td>bodyText</td></tr>
<tr><td>Type:</td><td>Richtext editor (使用搜索帮助你找到这个，选中所有默认的选项然后点击Submit)</td></tr>
<tr><td>Tab:</td><td>Contents</td></tr>
<tr><td>Description:</td><td>The main content of the page.</td></tr>
</table>

<table border="0">
<col width="130">
<col width="400">
<tr><th>Name</th><th>Footer Text</th></tr>
<tr><td>Alias:</td><td>footerText</td></tr>
<tr><td>Type:</td><td>Textbox</td></tr>
<tr><td>Tab:</td><td>Footer (记得添加这个!)</td></tr>
<tr><td>Description:</td><td>Copyright notice for the footer.</td></tr>
</table>

现在你创建好的文档类型，看起来是这样的：

![带属性的Homepage文档类型](images/figure-12-homepage-document-type-with-properties.png)

*图 12 - 带属性的Homepage文档类型*

我们现在创建好了第一个文档类型 - 

现在我们创建了第一个文档类型 - Umbraco 创建一个网页需要做三件事情，这是第一件也是最重要的一件。它可以把文档类型的数据存储在某个实例（Content）中，并将它们整合到模板中 - 接下来我们创建模板。

## 下一步 - [创建第一个模板和内容节点](Creating-Your-First-Template-and-Content-Node.md) #

如何创建第一个模板以及第一个内容节点。 