#设置和样式
网格布局还可以公开自定义设置 - 比如数据属性或者样式选项 - 对于每个单元格和行都是如此。它们允许编辑器使用一些友好的 UI 添加一些配置值到网格元素。当应用了自定义设置和样式时，她们将被作为html 属性或者内联样式被包含在网格 html 中。

![Grid layouts](images/settings.png)

在设置网格布局数据类型时，这些设置和样式必须由开发人员配置。

###配置自定义设置和样式
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

**key** defines the alias the configuration is stored under and by default the alias of the attribute will also be the attribute on the rendered html element. In the example above any value entered in this settings editor will be rendered in the grid html as:

    <div **class**="VALUE-ENTERED"></div>

By changing the key of the setting you can modify the `<div>` element's attributes like `class`, `title`, `id` or custom `data-*` attributes.


**view** the view defines the editor used to enter a value. By default Umbraco comes with a collection of prevalue editors:

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

Alternatively you can also pass in a path to a custom view like "/app_plugins/grid/editors/view.html"

**prevalues** is for views that need predefined values, e.g. the radiobuttonlist view. Prevalues are defined as strings in an array:
    
    "prevalues":[
        "value_1",
        "value_2",
        "value_3"
    ]

and will translate in to three different options where each string will become a radiobutton. The strings represent the value of the options.

**In Umbraco 7.5.4 & newer** prevalues can also be defined as an object of label/value allowing to have a displayed label instead of showing the actual underlying value. You can even mix and match these and use both label/value prevalues and simple string prevalues in the same configuration:

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

**modifier** is a basic way to prepend, append or wrap the value from the editor in a simple string. This is especially useful when working with custom styles which often requires additional values to function. For instance if you want to set a background image you can get an image path from the image picker view. But in order for it to work with css it has to be wrapped in `url()`. In that case you set the **modifier** to `url('{0}')` which means that `{0}` is replaced with the editor value.

**applyTo** defines what this setting can be applied to. It should be either **row** or **cell** as a string.

**In Umbraco 7.5.4 & newer** a JSON object can also be used if you need a more specific configuration. A JSON configuration could look like this:

    "applyTo": {
        "row": "Headline,Article",
        "cell": "4,8,6"
    }

This would ensure the setting can only be used on rows named **Article** or **Headline**, or on cells sized: **4**, **8** or **6**. If it should only apply to cells you can remove the row property. If it should apply to all rows you can specify it by having the row property with null or an empty string as value.

###Sample settings
There are many ways to combine these, here are some samples:

**Set a background image style**

    {
        "label": "Background image",
        "description": "Choose an image",
        "key": "background-image",
        "view": "imagepicker",
        "modifier": "url('{0}')"
    }


**Set a title setting**

    {
        "label": "Title",
        "description": "Set a title on this element",
        "key": "title",
        "view": "textstring"
    }


**Set a data-custom setting**

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

###Multiple settings and styles
You can add multiple settings and styles configurations on a datatype. This is done by creating a new setting or style object. Remember to separate the objects with a comma.

**Adding multiple settings**

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


###Full-width settings and styles
It is possible to use settings and styles to add full-width background-images, background-colors and so forth. Just make sure the surrounding *section* is full-width(12 columns by default) and the *rows* inside it will automatically become full-width.
