
# 数据类型 #
*一个数据类型定义了一个属性的输入类型。所以在你添加属性（在文档类型、媒体类型和会员中）时，当你选择了一种类型，就是选择了一个数据类型。在 Umbraco 中已有一些预先设定好的数据类型，也可以在Developer 板块中添加更多。*

## 什么是数据类型？ ##
一个数据类型可以非常简单（textstring, number, true/false,...），也可以非常复杂(multi node tree picker, image cropper, Grid Layout)。

数据类型引用自属性编辑器，如果属性编辑器包含一些设置也会被配置在数据类型中。这意味着你可以有多个数据类型引用自同样的属性编辑器。

一个例子是，可以有两个下拉列表数据类型都引用自同样的下拉列表属性编辑器。一个配置为城市列表，另外一个为州列表。

## 创建新的数据类型 ##
创建新的数据类型要前往开发区块，点击 __Data-Types__ 右边的菜单图标，然后选择 __Create__ 。命名数据类型，我们称之为"Dropdown Cities"。

![下拉列表](images/Data-Types-Create.jpg)

* __Property Editor:__ 这是你为我们创建的 *Dropdown Cities* 数据类型选择指向的属性编辑器的地方。选择 __Dropdown List__ 你会看到数据类型引用的下拉列表属性编辑器中一些可用的配置项。

* __Property Editor Alias__ 使用的属性编辑器别名。

* __Add prevalue:__ 这里你可以通过在输入框中输入你想要的值然后点击 __add__ 按钮给你的数据类型添加预设值。对于 "Dropdown Cities" 列表可以是：
    * London
    * Paris
    * Berlin

当你结束了列表输入后，点击 __Save__ 按钮，现在你就可以在文档类型，媒体类型和会员中，为属性选择这个数据类型。这将创建一个下拉列表供编辑器选择，并将该选项保存为字符。

## 自定义数据类型 ##
要自定义已有的数据类型，前往  __Developer__ ，展开 __Data-Types__ 节点，选择想要编辑的数据类型。

除了数据类型之外，还有些可用的额外属性编辑器用于 __Slider__ 和 __Macro Container__ 。

### 更多信息 ###
* [可用数据类型列表](default-data-types.md)
* [属性编辑器](../../Backoffice/Property-Editors/)

### 相关服务 ###
* [数据类型服务](../../../Reference/Management/Services/DataTypeService.md)

### Umbraco.tv ###
* [Episode: Data Types](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/document-types/data-types/)
* [Episode: Creating and Reusing Editors](https://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/document-types/creating-and-reusing-editors/)

