# 后台概述
这些条目在Umbraco 后台中是通用的术语和概念。

### [登录界面](Login/)
当你第一次进入后台，你会看到登录界面。
![Login screen](images/login.png "屏幕中间有问候语, username/password 字段和复选框，还有一个忘记密码链接。")


### [区块](Sections/)
Umbraco 区块是指一组相关的特殊任务组成的部分。例如内容、设置、开发。你可以在区块菜单中，点击相应的图标在不同区块之间跳转。

![区块](images/sections.jpg "The Section menu is the vertical menu located on the left side of the backoffice.")
*__区块菜单__是定位在后台左侧的菜单。*

### 树
树是与特定概念相关的（通常是受限关系）条目层次结构列表，它可能类似于内容树或媒体树。你可以点击节点左侧，向下的箭头<img src="images/expand-node.png" style="margin:0;width:15px" title="Expand a node in a tree" />展开树。[了解更多..](../../Extending/Section-Trees/index.md)
![Tree](images/tree.jpg "The content tree")
*内容树*

###节点
节点是树中的一个项目。Media 区块中的图片和目录显示为 Media 树中的节点，Content 区块中的页面和内容显示为 Content 树的节点诸如此类。

###控制台/仪表板
仪表板是你在进入后台中的区块看到的主要视图，可以用来向用户显示一些系统的有价值信息。
![Dashboard](images/dashboard.jpg "Default dashboard in the content section")
*默认显示的是Content区块的仪表板*
[了解更多..](../../Extending/Dashboards/index.md)

### 编辑器
编辑器用于在后台编辑不同的条目。这里有一些特殊的编辑器用于编辑样式表，有一些编辑器用于编辑宏等等。

###内容
内容是指在内容区块的页面和内容。树中的每一个条目都称之为节点。内容树中的每个节点都存在不同的字段。每一个内容条目（或节点）都是由文档类型所定义的。
[了解更多..](../Data/Defining-Content/)

###文档类型
文档类型定义了后台用户可以在内容树中创建的页面/内容类型。每个文档类型都包含不同的属性或字段。每个字段都包含特定的数据类型，例如：文本、数字……

###属性
每个文档类型都包含属性。这是内容编辑器中允许为节点编辑的字段。

###数据类型
文档类型中的每一个属性都包含一个数据类型，它定义了该属性的输入类型。数据类型指向一个属性编辑器，在Umbraco后台的开发区块中可以进行配置。

一个数据类型可以非常简单（textstring、number、true/false、……），或者比较复杂（multi node tree picker、image cropper、……）
[了解更多..](../Data/Data-Types/)

###[属性编辑器](Property-Editors/)
属性编辑器是向 Umbraco 插入内容的一种方式。富文本编辑器就是属性编辑器的一个实例。它可能会与数据类型混淆。可能有很多富文本编辑的数据类型使用了不同的设置，而他们都使用了富文本编辑类型的属性编辑器。

###媒体
Media 区块中的 Media 条目，用于存储类似于图片和视频一类的资源，并且可以从你的内容中引用它们。
[了解更多..](../Data/Creating-Media/)


###媒体类型
除了它是专门用于指定 Media 区块中的 media 条目之外，媒体类型与文档类型非常相似。

###会员
会员是指某个已经注册、登录并可以操作你的**公开网站**的人，不要与用户混淆
[了解更多..](../Data/Members/)

###会员类型
类似于文档类型和媒体类型。你可以定义一些自定义属性，用于存储会员的 twitter 账户、网站 URL 等。

###模板
模板是为你的网站定义 HTML 标记的地方。布局通常是通用的模板，包含一些通用的标记，类似于`<head>` 段落。
[了解更多](../Design/Templates/)

###包
package 是指能够扩展 Umbraco的模块或者插件的Umbraco 术语。关于包可以在这里找到[Umbraco项目](https://our.umbraco.org/projects/ "Projects on Our Umbraco").

###宏
宏是可重用的功能组件，它可以在你的整个站点中重复使用。宏可以配合参数配置，支持插入富文本编辑器。你可以定义哪些已有的宏可以在编辑器中使用。当编辑器中插入宏时，它会提示要输入已经定义好的参数。
[了解更多..](../../Reference/Templating/Macros/)

###宏参数编辑
参数编辑器定义属性编辑器的用法，作为宏的参数使用。
[了解更多..](../../Extending/Macro-Parameter-Editors/)

###用户
用户是指可以操作**Umbraco后台**的人，不要与会员混淆。在安装 Umbraco 时，会使用输入的 login、email 和密码生成用户。用户可以在 User区块中创建、编辑以及管理。
