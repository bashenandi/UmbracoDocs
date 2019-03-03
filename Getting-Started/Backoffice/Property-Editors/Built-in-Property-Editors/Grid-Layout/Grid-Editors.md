# 网格编辑器 #

网格编辑器是负责将数据输入网格的组件 - 可以是简单的文本字段或者媒体选择器。它们的构建方式与属性编辑器相同，因此由3部分组成：

- .html 视图文件
- .js 控制器
- .cshtml 服务器端渲染

视图是编辑者所看到的东西，控制器处理它的行为，CSHTML决定如何在模板中呈现输入的数据。


### Default Grid editors ###
默认的编辑器在`/config/grid.editors.config.js`中声明。它们是以JSON格式编写的，每个编辑器都是类似这样的对象：

    {
        "name": "Rich text editor",
        "alias": "rte",
        "view": "rte",
        "icon": "icon-article"
    }

### Custom Grid editors ###
您可以轻松地根据需要来自定义内置编辑器来适应您的网格。

##### package.manifest ####
当你定义自己的编辑器时推荐使用`package.manifest`文件（并不是上面介绍的配置文件中）例如：

    {
        "gridEditors": 
        [
            {
                "name": "Rich text editor",
                "alias": "rte",
                "view": "rte",
                "icon": "icon-article"
            }
        ]
    }
    
不同于`/config/grid.editors.config.js`文件中网格编辑器的JSON 根元素是一个数组，在`package.manifest`文件中是从一个具有多个不同属性的json对象开始，其中一个属性是`gridEditors`。

打包文件清单位于`/App_Plugins/`目录中的某个目录里面 - 比如`/App_Plugins/{YourPackageName}/package.manifest`。您可以定义任意多个网格编辑器，并且可以在多个清单上进行定义，这样您就可以使用清单文件中的网格编辑器。在`package.manifest`文件就位后，Umbraco将在启动过程中自动提取它。

你可以在[Package Manifest](../../../../../Extending/Property-Editors/package-manifest.md)页面读到更多关于`package.manifest`的信息。

##### Grid editor configuration #####

对于一个网格编辑器，必要的值包括：

- **name**: 编辑器的名字
- **alias**: 唯一的编辑器别名
- **icon**: 显示在编辑器中的图标，使用与其他图标相同的图标类
- **view**：view 定义了编辑者可以输入的值。Umbraco默认会在 `/umbraco/views/propertyeditors/grid/editors`中查找 htmlm 视图来使用，但是你也可以使用自己的路径来替换

你可以使用的内置视图：

- textstring
- rte
- embed
- macro
- media

在大多数情况下，您要么使用文本字符串或媒体视图，要么从头开始构建自己的视图。textstring和media编辑器提供了一些额外的配置，以方便快速地自定义它们。

##### Sample textstring config #####

    {
        "name": "Headline",
        "alias": "headline",
        "view": "textstring",
        "icon": "icon-coin",
        "config": {
            "style": "font-size: 36px; line-height: 45px; font-weight: bold",
            "markup": "<h1>#value#</h1>"
        }
    }

在此示例中，`config.markup`值应用于编辑器，以便用户可以在后台看到准确的预览。这将作为内联样式应用于后台的文本区域。`config.markup`会作为字符串输出到模板中。其中的`#value#`会被真实的值所替代。

##### Sample media config #####

    {
        "name": "Square Image",
        "alias": "squareImage",
        "view": "media",
        "icon": "icon-picture",
        "config": {
            "size": {
                "height": 200,
                "width": 200
            }
        }
    }

在此示例中，`config.size`将根据`height` 和 `width`调整图像大小。上面的示例将导致渲染图像为200x200像素，无论上传图像的大小如何。如果大小比与上载的图像不同，则可以设置一个焦点，以确定图像应如何裁剪。

![Resizing](images/grid-resizing.png)
