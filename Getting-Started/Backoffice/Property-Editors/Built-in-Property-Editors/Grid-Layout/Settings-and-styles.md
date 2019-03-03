# 设置和样式 #

网格布局还可以暴露自定义设置 - 比如数据属性或者样式选项 - 对于每个单元格和行都是如此。它们允许编辑器使用一些友好的 UI 添加一些配置值到网格元素。当应用了自定义设置和样式时，她们将被作为html 属性或者内联样式被包含在网格 html 中。

![Grid layouts](images/settings.png)

在设置网格布局数据类型时，这些设置和样式必须由开发人员配置。

### 配置自定义设置和样式 ###
为了增加设置，点击*Edit Settings*链接。会展开一个显示原始数据的对话框。这些数据是 JSON 格式的，并且在保存会验证 JSON 格式。

设置数据看起来大概是这样的，每项设置都是一个对象:

	[
		{
			"label": "Class",
			"description": "Set a css class",
			"key": "class",
			"view": "textstring",
			"modifier": "col-sm-{0}",
			"applyTo": "row|cell"
		}
	]

这些值表示：

- **label** : 在内容编辑 UI 中显示的字段名
- **description** : 在内容编辑UI 中显示的描述用于指导用户使用
- **key** : 输入的值将根据 key 来存储
- **view** : 用于给编辑器输入一个设定值
- **prevalues** : 对于某些 views 需要预先定义的值，例如radiobuttonlist
- **modifier (optional)** : 一个格式化字符串用于输出预先定义的值或者附加的额外值
- **applyTo (optional)** : 此设置应用于行还是单元格。如果这个值不存在或者为空，此设置将被同时应用于行和单元格。

**label** 和 **description** 最好描述清晰直接了当。

**key**定义了配置的存储别名，并且在渲染 html 元素时默认将别名作为属性名。下面的例子中，任何在这个配置编辑中输入的值都会输出到网格 html 中：

    <div **class**="VALUE-ENTERED"></div>

想要修改设置的键，你可以编辑`<div>`元素的属性，例如`class`, `title`, `id`以及自定义的`data-*`属性。

**view**定义了编辑者可以输入的值。默认情况下，Umbraco附带了一组预设的编辑器：

- textstring
- textarea
- radiobuttonlist
- mediapicker
- imagepicker
- boolean
- treepicker
- treesource
- number
- multivalues

或者你也可以使用类似于"/app_plugins/grid/editors/view.html"这样的自定义视图的路径来替代。

**prevalues**用于视图所需的预设值，例如单选按钮列表视图。预设值定义为一个字符串类型的数组：
    
    "prevalues":[
        "value_1",
        "value_2",
        "value_3"
    ]

这将会把每个字符串转换成为三个不同的选项的单选按钮。显示的字符串就是选项的值。

**从Umbraco 7.5.4 版本开始**，预设值可以定义为label/value 的对象，允许有一个显示的标签，而不是实际值。你还可以在同一个配置项中混合使用 label/value 预设值和简单的字符串预设值：

    "prevalues":[
        {
            "label": "Value one",
            "value": "value_1"
        },
        {
            "label": "Value two",
            "value": "value_2"
        },
        "value_3"
    ]

**modifier**是可预置的，将编辑者输入的值添加或者嵌入到一个简单字符串中的基本方法。这在处理平时需要附加值才能工作的自定义样式时特别有用。举例来说如果你想设置一张背景图片，你可以从图片选取器中获取到图片的路径。但是为了让它在 css 中能正常工作，必须包裹在`url()`中。这种时候，你就可以把**modifier**设置为`url('{0}')`，这代表着`{0}`将会被编辑者选择的值所替代。

**applyTo**定义了这些设置可以被应用在哪里。它只能是作为字符串的**row** 或者 **cell**。

**在 Umbraco 7.5.4 以及更新的版本中** 如果你需要更多特殊的配置，也可以使用 JSON 对象。它看起来是下面这样的：

    "applyTo": {
        "row": "Headline,Article",
        "cell": "4,8,6"
    }

这会使得该设置仅应用于名称为**Article** 或者 **Headline**的rows或者尺寸为**4**, **8** 或者 **6**的 cells。如果你只需要应用于 cells 你可以移除 row 属性。如果要应用于所有 rows 你可以将 row 属性设置为 null 或者一个空的字符串作为值。

### 设置实例 ###

有很多方法可以组合使用这些，这里有一些示例：

**设置一张背景图片的样式**

    {
        "label": "Background image",
        "description": "Choose an image",
        "key": "background-image",
        "view": "imagepicker",
        "modifier": "url('{0}')"
    }


**设置标题配置**

    {
        "label": "Title",
        "description": "Set a title on this element",
        "key": "title",
        "view": "textstring"
    }


**设置一个数据自定义配置**

    {
        "label": "Custom data",
        "description": "Set the custom data on this element",
        "key": "data-custom",
        "view": "radiobuttonlist",
        "prevalues": [
            "value_1",
            "value_2",
            "value_3"
        ]
    }

### 多配置和样式 ###
你可以添加多个设置和样式设置到一个数据类型。这是通过添加新的setting 或 style 对象来实现的。记住对象之间用逗号分隔。

**添加多个设置**

    [
        {
            "label": "Class",
            "description": "Set a class on this element",
            "key": "class",
            "view": "textstring"
        },
        {
            "label": "Title",
            "description": "Set a title on this element",
            "key": "title",
            "view": "textstring"
        },
        {
            "label": "Custom data",
            "description": "Set the custom data on this element",
            "key": "data-custom",
            "view": "textstring"
        }
    ]


### 全宽设置和样式 ###

可以使用setting和styles 来添加全宽背景图像、背景颜色等。只需确保外层的*section*是全宽的（默认为12列），其中的*rows*将自动变为全宽。