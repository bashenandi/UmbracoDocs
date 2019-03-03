# 内置 Umbraco 属性编辑器 #

本页包含了所有内置的 Umbraco 属性编辑器清单，以及他们能做什么的简短描述：

## [复选框列表](CheckBox-List.md) ##

`Alias: Umbraco.CheckBoxList`

将预设值显示为复选框控件列表

## 颜色选择 ##
`Alias: Umbraco.ColorPickerAlias`

添加可通过点击选择的可以显示的颜色列表

## [内容选取](Content-Picker2.md) ##
`Alias: Umbraco.ContentPicker2`
内容选取允许内容编辑器从内容结构中选取特定的节点

## [日期](Date.md) ##
`Alias: Umbraco.Date`
显示一个日历控件用于选择日期

## [日期/时间](Date-Time.md) ##
`Alias: Umbraco.DateTime`

显示一个日历控件用于选择日期和时间

## 十进制整数 ##
`Alias: Umbraco.Decimal`

一种可配置的数字控件，只允许数字包括小数。

## [下拉列表](Dropdown.md) ##
`Alias: Umbraco.DropDown.Flexible`

从 Umbraco v7.10开始引入。显示一个预设值的列表。内容编辑器可以从其中选择一个或者多个值。

## 邮件地址 ##
`Alias: Umbraco.EmailAddress`

一个简单的单行文本，仅允许有效的邮件地址。

## 文件上传 ##
`Alias: Umbraco.UploadField`

添加一个文件上传字段，允许上传文档或图片到 Umbraco

## 文件夹浏览
`Alias: Umbraco.FolderBrowser`

主要用于媒体类型容器，文件夹浏览器显示缩略图列表。

## [网格布局](Grid-Layout.md)
`Alias: Umbraco.Grid`

v7.2版本新引入，给编辑者一个网格编辑器，允许他们在预定义布局中插入不同类型的内容。


## [图片裁剪](Image-Cropper.md)##
`Alias: Umbraco.ImageCropper`

用于将图片裁剪和调整为预设值大小。v7.1以后版本可用

## 标签 ##
`Alias: Umbraco.NoEdit`

标签是不可编辑的控件，仅用于显示预先设置的文本。

## 旧媒体选取器 ##
`Alias: Umbraco.MediaPicker`

传统媒体选取器会打开一个简单的对话框用于从媒体树中选取指定的媒体条目。保存值是选取的媒体的 ID。

## 列表视图 ##
`Alias: Umbraco.ListView`

此控件提供与标准ListView相同的功能，但允许您将ListView作为控件添加到选项卡上，同时控制其他选项卡和属性。

## 宏容器 ##
`Alias: Umbraco.MacroContainer`

宏容器在早期是唯一一种允许重复内容的内置方式。它允许内容编辑器添加多个块。每个块都是所选宏的一个，包含XSLT、用户控件或宏部分。如今，在不同的包中，还有其他选项可以重复包含内容，如[Nested Content](Nested-Content.md), [Grid Layout](Grid-Layout.md)和许多其他控件。

## Markdown 编辑器 ##
`Alias: Umbraco.MarkdownEditor`

[Markdown](https://daringfireball.net/projects/markdown/) 是一种具有纯文本格式语法的轻量级标记语言。它的设计可以将其转换为HTML。内置编辑器允许用户使用标记格式选项。

标记编辑器将由模型生成器解释。在幕后，Umbraco使用了[Markdown NuGet package](https://www.nuget.org/packages/Markdown/)。

## [媒体选取器](Media-Picker2.md) ##
`Alias: Umbraco.MediaPicker2`
媒体选取器会显示当前选择的媒体文件，并且提供选项用于打开媒体选择对话框，选择已有的或者上传新的媒体文件。有一些设置，可以设置允许多选媒体的条目。

## 会员组选取器 ##
`Alias: Umbraco.MemberGroupPicker`

## [会员选取器](Member-Picker.md) ##
`Alias: Umbraco.MemberPicker2`


## [多节点树选取器](Multinode-Treepicker2.md) ##
`Alias: Umbraco.MultiNodeTreePicker2`

多节点树选取器数据类型，允许内容编辑器在内容树或者媒体树中选取多个节点。

## [多文本框](Multiple-Textbox.md) ##
`Alias: Umbraco.MultipleTextstring`

多文本框属性编辑器允许在内容编辑器中生成一组文本条目的列表。

## [嵌套的内容](Nested-Content.md) ##
`Alias: Umbraco.NestedContent`

v7.7版本新引入，嵌套内容属性编辑器允许你为列表条目使用文档类型架构。

## 数字 ##
`Alias: Umbraco.Integer`

一种可配置的数字控件，只允许整数。

## [单选按钮列表](RadioButton-List.md) ##
`Alias: Umbraco.RadioButtonList`

正如其名称所表明的，这个属性编辑器允许编辑器从单选按钮列表中进行选择。

## [相关链接](Related-Links2.md) ##
`Alias: Umbraco.RelatedLinks2`

相关链接允许编辑器简易的添加链接数组。数组内任一元素可以是 Umbraco 内部链接或者外部 URLS。

## 富文本编辑器 ##
`Alias: Umbraco.TinyMCEv3`

基于[tinymce](https://www.tinymce.com/)的高度可定制化富文本编辑器。可能是 Umbraco 项目中使用最多的控件。

## Slider ##
`Alias: Umbraco.Slider`

具有一定范围内的数字的滑块。

## [Tags](Tags.md) ##
`Alias: Umbraco.Tags`

可以由一组特定的标记控制的标记控件。

## [Textarea](Textarea.md) ##
`Alias: Umbraco.TextboxMultiple`

一个简单的文本域，用于输入文本。

## [Textbox](Textbox.md) ##
`Alias: Umbraco.Textbox`

常规的 html 文本输入字段。

## [True/False](True-False.md) ##
`Alias: Umbraco.TrueFalse`

一个简单的复选框，可以保存0或者1，这取决于复选框是否被选中。

## User picker ##
`Alias: Umbraco.UserPicker`

从用户后端用户中选择人员的最简单方法。请参阅前端用户的成员。