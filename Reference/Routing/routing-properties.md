# 路由的特殊属性类型别名 #

_有一些特殊/保留的Umbraco属性类型别名可以轻松使用操作标准Umbraco路由管道的工作方式。你可以添加这些属性类型到任何文档类型，并且如果提供了这些属性的值，Umbraco 将相应地调整其路由。详情见下文。_

## umbracoRedirect

使用此名称创建属性别名并使用内容选取器属性编辑器，可以创建302临时重定向。

这意味着当用户导航到此节点时，他们将被重定向离开该节点。

## umbracoInternalRedirectId

使用内容选取器属性编辑器将此属性别名添加到文档类型，然后Umbraco将透明地加载所选页面的内容，而不执行任何URL重定向。
这本质上执行了重写。

## umbracoUrlName

此属性创建为文本字符串时，允许您为默认由节点名称创建的内容提供不同的URL名称。

如果输入此属性的值并保存/发布内容节点，则会看到其主URL已用新的路径后缀更新。

## umbracoUrlAlias

当创建为文本字符串时，此属性允许您提供逗号分隔的节点的备用完整URL路径列表。例如，如果您的URL是/some category/some page/content节点，通过添加“flowers”的umbracourlalias，用户只需转到/flowers即可导航到节点。

URL别名在浏览器地址栏中保留为实际URL的“掩码”。您还可以指定“花/玫瑰/红色”等路径。

## Filtering

[请参见过滤属性惯例](../Querying/IPublishedContent/Collections.md#filtering-conventions)
