# 输出文档类型属性 #

你应该注意到，我们在 homepage 中添加的内容，并没有输出出来。我们需要需要将数据类型属性（我们在Umbraco 中创建，可以进行编辑的数据字段）和模板打通。让我们看看我们的模板，并确定好我们之前创建的数据属性中的内容应该放在什么位置。



![我们的数据属性内容要输出到哪里](images/figure-17-where-our-data-fields-go.png)

*图 17 - 我们的数据属性内容要输出到哪里*

我们用蓝色标出了想要输出数据属性内容的地方。现在我们需要打通相关属性。

前往 **_Settings > Templates > Homepage_**。向下滚动并且选中文字`“h1#title”`，大约在27行前后。

![准备用 Umbraco 数据属性替换硬编码文本](images/figure-18-replace-hardcoded-text-with-umbraco-page-field.png)

*图 18 - 准备用 Umbraco 数据属性替换硬编码文本*

点击按钮 **_Insert Umbraco Page Field_** ，在 **_Choose field_** 下拉列表中的**_Custom Fields_**部分，选中**_pageTitle_**。

![Umbraco 页面字段/属性](images/figure-19-umbraco-page-field.png)

*图 19 -  Umbraco 页面字段/属性*

点击绿色的**_Insert_**按钮之后再点击**_Save_** 按钮。

接下来用同样的方法使用字段**_bodyText_**替换`<header></header>`标签（大约在42-43行）中的内容。再次点击**_Insert_**按钮和**_Save_** 按钮。

![使用Umbraco 页面字段替换文本正文](images/figure-20-replace-bodytext-with-page-field.png)

*图 20 - 使用Umbraco 页面字段替换文本正文*

最后要替换的是 footer - 在底部 div （68行）中的`<h3></h3>`标签。

![底部文字](images/figure-21-footer-text.png)

*图 21 - 底部文字*


现在我们去重新加载首页……啊哈！我们有了正确内容！现在，我们返回去添加两个选项卡，称为Article 1，Article 2和Article Footer，每个标签都包含标题和内容字段，并将这些代码放在模板中的相关位置。如果在 homepage 中只有这两部分，这是可行的，或者我们也可以使用子节点来替换它们，从而变得灵活 - 我们将在后面了解到这些内容。

## 下一步 - [创建主模板 - 第一部分](Creating-Master-Template-Part-1.md) ##
如何创建主模板并利用它创建更多的页面，同时在你源文件中最小化地重复 HTML 代码。