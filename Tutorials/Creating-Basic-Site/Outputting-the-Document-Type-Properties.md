# 输出文档类型属性
你应该注意到，我们添加的到 homepage 的内容，并没有输出。我们需要需要将数据类型属性（数据字段我们在Umbraco 中创建，可以进行编辑）和模板打通。操作之前先看一下我们的模板，并且标记出我们需要显示数据属性的地方。

![Where our Data Properties Content Should be Output](images/figure-17-where-our-data-fields-go.png)

*图 17 - 我们的数据属性内容要输出到哪里*

我们用蓝色标记出了想要输出数据属性内容的地方。现在我们需要打通相关属性。

前往 **_Settings > Templates > Homepage_**。向下滚动并且选中文字`“h1#title”`，大约在27行。 

![Preparing to replace the hardcoded text with an Umbraco Page Field](images/figure-18-replace-hardcoded-text-with-umbraco-page-field.png)

*图 18 - 准备使用 Umbraco 数据属性替换硬编码文本*

点击按钮 **_Insert Umbraco Page Field_** 在 **_Choose field_** 下拉列表的**_Custom Fields_**部分选中**_pageTitle_**。

![Umbraco Page Field](images/figure-19-umbraco-page-field.png)

*图 19 -  Umbraco 页面字段和属性*

点击绿色的**_Insert_**按钮之后再点击**_Save_** 按钮。

接下来用同样的方法使用字段**_bodyText_**替换`<header></header>`标签（大约在42-43行）中的内容。再次点击**_Insert_**按钮和**_Save_** 按钮。

![Replacing the bodyText with the Umbraco Page Field](images/figure-20-replace-bodytext-with-page-field.png)
*图 20 - 使用Umbraco 页面字段替换主要内容*

最后我们操作 footer - 在底部 div （68行）中的`<h3></h3>`标签。

![Replacing the Footer Text with the relevant Umbraco Page Field](images/figure-21-footer-text.png)
*图 21 - 底部文字*


现在去重新加载你的首页……哈！我们有了内容！现在，我们返回去添加两个标签，称为Article 1，Article 2和Article Footer 每个都包含标题和内容字段，将这些放在模板中的相关位置。如果在 homepage 中这里只有两个区块，是可以正常显示的，我们还可以使用子节点，替换它们使其变得灵活 - 我们将在后面学到这些。


---
##下一步 - [创建主模板 - 第一部分](Creating-Master-Template-Part-1.md)
如何创建主模板和使用它创建更多的页面，同时从你的资源文件中复制更少的服务 Html 代码。

