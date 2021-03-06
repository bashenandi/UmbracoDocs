# 定义内容 #

*这里你可以找到如何定义内容和第一次如何开始的快速指南说明(基于空白的安装)。*

在创建一块内容之前，需要先定义它。当你打开一个空白的 Umbraco 安装程序，它无法在_Content_区块创建内容，这是为什么？所有内容都需要一个蓝图结构来知晓这个内容节点可以存储什么类型的数据，有什么编辑器可以使用，如何组织它们，允许什么样的结构等等。这个蓝图或者定义，称之为文档类型。

## 什么是文档类型? ##
文档类型作为一个表单容器字段集合（或者标签页），其中包含很多基础的表单，你可以定制规则来约束什么样的内容可以被创建，可以允许使用什么样的模板，后台图标等等。

文档类型能够定义整个页面或者局部内容，用于在其他节点中重用，例如 SEO 标签。这意味这你可以完全控制什么类型的内容可以被创建在哪里。

### 属性 ###
文档类型中的每个字段称之为属性。属性需要一个名称，一个别名（用于在模板中输出属性内容）和一个编辑器。编辑器决定了这个属性存储什么样的数据以及输入方式。这里有大量的编辑器可以使用（文本框，富文本，媒体选择等等），并且你也可以定制他们，还可以添加附加的编辑器。

有些编辑器需要配置后才能使用，一个配置好的编辑器会被保存为一个数据类型，它可以在多个属性和多个文档类型中使用。这些可以在__开发区块__下面的__数据类型看到__。

## 创建一个文档类型 ##
在__Settings__板块中使用文档类型编辑器来创建一个文档类型。

前往后台的__Settings__板块。在__Document Types__节点上点击菜单图标(•••)展开上下文菜单。这里选择__Document Type__。这会创建一个带模板（可以在__Settings__区块下的__Templates__中找到）的文档类型，同时指派它为这个文档类型的默认模板。

![创建文档类型](images/Document-Type-Create.jpg)

_你也可以选择创建一个不带模板的文档类型和创建一个目录来组织你的文档类型。_

你还可以使用组件来创建一个新的模板类型。组件允许你从其他标签中继承属性。使用混合设置时，可以通过访问结构选项卡来使用嵌套的组件。样式类似于：

![Creating a Compositions](images/Doc-Type-Composition-Create.png)

灰色显示的文档类型组件**Master**是我们正在查看的特定文档类型的父级。默认情况下，这意味着此文档类型将继承**Master**文档类型的属性，除非将其移动到其他位置，否则它将保持不变。选中的另一个文档类型是**Banner**类型。这意味着文档还将从**Banner**类型继承属性到该文档类型。

### 定义根节点 ###
首先我们要给文档类型一个名字。这第一个文档类型，会成为我们所有内容的根节点，所以命名为"Home"。

![文档类型命名](images/Document-Type-Name.jpg)

_注意，文档类型的别名会基于名字自动生成。如果你想修改这个别名，点击旁边的"锁"图标。_

有了一个根节点，会让内容查询变得更加简单，你要知道所有内容都应该在根节点的下面。

为了给文档类型设置一个图标，需要点击左上角的文档图标。这会打开一个图标选择对话框。搜索_Home_并且选择这个图标。这个图标会被是用在内容树中，为你的内容节点选择一个合适的图标，会在内容树的呈现上给编辑者更好的体验。

![给文档类型选择图标](images/Document-Type-Choosing-Icon.jpg)

前往__Permissions__标签，勾选__Yes - allow content of this type in the root__复选框，然后点击右下角的保存按钮保存这个文档类型。

![允许在根节点](images/Document-Type-Allow-At-Root.jpg)

### 创建根节点 ###
现在前去__Content__区块，点击__Content__上菜单图标，选择"Home"文档类型。我们给它命名为"Home"，然后点击__Save and Publish__按钮。

![创建了第一个内容](images/Document-Type-Root-Node-Created.jpg)

由于我们并没有创建任何属性，因为我们能够看到的只有"Home"节点的Properties标签页，它包含了Umbraco 中所有内容包含的默认属性。

现在我们去添加一些属性。

### 标签页和属性  ###
前去__Settings__板块，点击左侧的箭头展开__ Document Type__，然后选择其中的__Home__文档类型。

#### 添加标签页 ####
在我们开始给文档类型添加属性之前，我们需要先创建一个标签页来管理这些属性。

点击 __Add new tab__ ，为其命名"Content"。

![创建标签页](images/Document-Type-Create-Tab.jpg)
_如果你有多个标签包含一个或多个属性，你可以通过拖拽或者直接输入数字来进行这些排序。你需要点击**Reorder**来完成这些。_

#### 添加属性 ####
现在我们创建好了标签页之后，就能开始添加属性了。让我们在 Content 标签页中添加一个富文本编辑器。

点击Content 标签页中的__Add property__链接。会打开属性设置对话框。这里你可以给每个属性设置元数据（name,alias,description），选择一个数据类型/属性编辑器使用，如果需要还可以添加数据验证。

给属性起一个名字，这个名字会在编辑器中显示，因此要有一定相关性或易于理解。注意别名会基于名字自动生成。我们将其命名为 "Body Text"。

![添加属性](images/Document-Type-Adding-Properties.jpg)

##### 键盘快捷键 #####
当你在文档类型编辑器中操作时，有一些键盘快捷键可供使用。同时按下<kbd>ALT</kbd> + <kbd>SHIFT</kbd> + <kbd>K</kbd>查看可供使用的快捷键：

![键盘快捷键](images/Document-Type-Keyboard-Shortcuts.jpg?width=400)

##### 属性编辑器 #####
点击__Add editor__会打开_Select editor_对话框。在这里你可以在__Available editors__（这会创建一个新的配置）中选择所有的，或者在__Reuse__中选择所有已经配置好的编辑器。为了是其更加容易查找，你可以使用在搜索框中输入"Rich"来进行过滤。过滤器首先会显示已配置的属性（在 Reuse 下面）和之后的可用编辑器。

在可用编辑器下面选择__Rich Text editor__。

![选择富文本编辑器](images/Document-Type-Rich-Text-Property.jpg)

这里会给你一些配置用于设置编辑器 - 这就是富文本编辑器的属性。注意数据类型的名字（_Home - Body Text - Rich Text editor_）是基于文档类型的名字，属性的名字和属性编辑器的名字。让我们将其重命名为"Simple Rich Text editor"并且仅选择一些必要的选项。

* _bold_
* _italic_
* _alignLeft_
* _alignCenter_
* _link_
* _umbMediaPicker_

当你完成这些设置后，点击__Submit__。

勾选__Mandatory__复选框会使属性成为必填项，如果没有输入值则内容不能保存（在这里是指输入到 Richtext）。你还有一个选择，是在__Validation__添加正则表达式，用于额外的验证。

提交属性设置并且保存文档类型。如果你再次前去__内容__区块，并且点击 Home 节点，应该会看到包含了 _Body Text_ 属性的 _Content_ 标签页。

### 定义子节点 ###
下一步，我们创建一个简单文本页面的文档类型，用于网站的子页面使用。

返回 __Settings__ 区块，创建一个新的文档类型，命名为"Text Page"。添加一个标签页称之为"Content"，这次我们添加两个属性。先添加一个名为"Summary"的属性使用 __Textarea__ 编辑器，再添加一个属性称为"Body Text"，再次使用 __Simple Rich Text Editor__ 数据类型。

### 创建子节点 ###
为了能够在 __内容__ 区块创建 _Text Page_ ，我们需要允许 _Text Page_ 文档类型可以作为 _Home_ 节点的子节点被创建。选择 _Home_ 文档类型，然后打开 __Permissions__ 标签页。点击 __Add child__ 选择 _Text Page_ 。

![允许的子节点](images/Document-Type-Allow-Child-Node.jpg)

前去 __内容__ 区块然后点击 *Home* 节点上的菜单图标(•••)，然后选择 *Text Page* 文档类型。我们把这个页面命名为"About us"。现在我们就有了一个非常基础的内容结构。

![基础内容结构](images/Document-Type-Child-Node-Created.jpg)

文档类型的使用非常灵活，从定义可重用的区块到整个页面，有非常多的使用方法，还可以充当容器或存储仓库。


### 更多信息 ###
- [输出内容](../../Design/Rendering-Content/)
- [定制数据类型](../Data-Types/index.md)

### 相关服务 ###
- [内容服务](../../../Reference/Management/Services/ContentService.md)
- [内容类型服务](../../../Reference/Management/Services/ContentTypeService.md)


### 教程 ###
- [用 Umbraco 创建一个基础网站](../../../Tutorials/Creating-Basic-Site/)

### [Umbraco TV](https://umbraco.tv) ###
- [Chapter: Document Types](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/document-types/what-is-a-document-type/)
- [Chapter: Creating content](https://umbraco.tv/videos/umbraco-v7/content-editor/basics/creating-content/)