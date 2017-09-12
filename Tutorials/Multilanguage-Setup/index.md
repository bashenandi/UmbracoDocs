# 多语言网站#
_本部分描述的提示是如果给你的文档类型翻译标签文版，例如标签页、名字和描述。想了解更多关于如何创建多语言的前端网站，请阅读这篇文章[Umbraco中的多语种网站
](http://24days.in/umbraco/2012/multilingual-1-1/)。_

##多语言后台##
在 Umbraco 中，想要翻译文档类型中的标签页，名称和描述，可以使用词典。你还可以翻译你的文档名称。

翻译这些标签相当的简单。请注意文档类型的下列内容

**Tab**: Content<br/>
**Name**: Header<br/>
**Description**: This is the header of page.

文档类型的名字是"Document"。

如果你为这些字段创建了翻译为其他语言的词典条目，你就可以直接引用它们，然后那些标签会显示为和你的编辑者登录 Umbraco 语言一致的文本。因此你只要简单的引用词典的名字，同下面一样：

**Tab**: #Content<br/>
**Name**: #Header<br/>
**Description**: #HeaderDescription

文档类型的名字可以通过引用 #Document

这非常简单，不是麽？:)

###重要!###
当撰写本文时，Umbraco 的默认语言是 en-us，而 administrator 账户的语言设置为 en-uk。如果你尝试上面的操作，示例仅仅会在名称标签上显示[#Header]，这会造成一些混乱。为此，你可以将administrator账户的语言设置为 en-us。这对所有低于 Umbraco6.1.1的版本都是必要的操作。
