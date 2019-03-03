
#创建媒体 #
Umbraco 中的媒体处理和内容的处理方式差不多。只不过为媒体条目定义的不是文档类型，而是媒体类型。不同于一般的内容，有三种默认的媒体类型：

- Folder
- Image
- File

__Folder__ 媒体类型是在媒体树中组织媒体条目的容器。 __Image__ 媒体类型用于上传和存储图片类型的文件到媒体区块， __File__ 媒体类型用于上传和存储其他类型的文件到媒体区块中。这意味着你不必定义你自己的媒体类型就可以开始使用这部分。你已经有了组织和上传媒体的工具。

### 创建文件夹 ###
在开始之前为媒体条目创建专属文件夹是一个好习惯。确保你文件夹的名字简单易懂，可以帮助编辑者上传文件和图片到正确的地方。

创建媒体文件夹，要前往 __媒体__ 区块然后点击 __媒体__ 节点右边的菜单图标，你还可以右击 __媒体__ 节点然后选择创建。接着会打开创建对话框。选择 __Folder__ ，输入一个名字然后点击 __save__ 。

###上传图片和文件 ###
有几种不同的方式来做这件事。你可以像创建目录一样使用上下文菜单。点击目录的菜单图标之后选择 __Image__ 或者 __File__ ，为你的媒体条目输入名字后点击 __Choose File__ 按钮。

一个更简单的方式是拖拽文件到上传区域。如果是文件或图片，Umbraco 会自动检测到并在目录中创建媒体条目。你甚至可以拖拽整个目录（包含子目录），文件夹和文件结构将被重建。你还可以点击 __- or click here to choose files__ ，通过标准的 OS 文件选取对话框来上传。

当图片文件上传时，默认的 __Image__ 媒体类型会自动填充5个属性。他们是： __Upload Image__, __Width__, __Height__, __Size__ and __Type__ ，可以在 __媒体__ 区块中浏览和在模板中操作。

### 组合和编辑媒体条目 ###
媒体区块的默认视图是卡片视图，可以让你预览图片和文件。

![媒体区块 - 卡片视图](images/Creating-Media-Cardview.jpg)

你可以通过点击图片选择多个媒体条目，并进行批量操作（删除/移动）。可以通过点击条目底部的蓝条来编辑媒体条目的属性。

![编辑媒体条目](images/Creating-Media-Edit.jpg?width=200px)

你可以通过点击视图切换选择列表视图来切换到列表的显示方式，切换按钮右侧是搜索框。

![媒体区块 - 列表视图](images/Creating-Media-Listview.jpg)

###在内容中使用媒体条目 ###
通过将一个 __Media Picker__ 属性添加到文档类型，编辑器就可以在 __内容__ 区块中选择媒体条目。

_提示：图片的 **Upload File** 属性使用了图片裁剪数据类型(Image Cropper Data Type)。如果添加了裁剪器，你可以指定媒体条目上使用的裁剪数据并且在模板中操作他们。通过设置区块的图片媒体类型或者开发区块下面的数据类型，你可以编辑上传文件属性添加裁剪器。_

## 创建媒体类型 ##
你可以创建你自己的媒体类型并添加标签页、属性和控制媒体树中的结构，如同文档类型一样。这意味着你可以在媒体条目中存储特定的媒体信息。

媒体类型在 __设置__ 区块中，使用媒体类型编辑器创建。

前往 __设置__ 区块。在 __Media Types__ 上点击菜单图标（或者右击节点）打开上下文菜单。这里你可以在创建媒体类型和目录之间选择。

_提示：针对不同的媒体类型使用不同的文件夹，可以限制媒体项目的创建位置。例如仅允许PDF上传到某个文件夹而员工图片上传到另一个位置，这样对编辑人员来说更容易保持媒体部分的组织性。_ 

选择 __New Media Type__ 打开媒体类型编辑器。它和创建文档类型的编辑器基本一样，不同之处在于对于媒体区块来说，有不同的媒体类型定义条目，以及你不能给媒体类型指定可用的模板。  

![创建媒体类型](images/Creating-Media-Create-740.jpg)

将媒体类型命名为"Employee Image"。通过点击名称左边的图标，选择一个图标（用户图标）。

#### 添加标签页 ####
在我们开始给媒体类别添加属性之前，需要先创建标签页来把它们放进去。

点击 __Add new tab__ 创建一个名为"Image"的标签。

#### 添加属性 ####
我们需要添加一些类似于默认的 __Image__ 媒体类型的属性。他们是：

- umbracoFile
- umbracoWidth
- umbracoHeight
- umbracoBytes
- umbracoExtension

在 Image 标签中点击 __Add new property__ 。命名为 "Uplaod image"，更改别名为"umbracoFile"。

点击 __Add editor__ ，搜索 "cropper"并且选择 __Available editors__ 下面的 __Image cropper__ 。这样会创建一个新的图片裁剪数据类型(Image Cropper Data Type)。默认生成的数据类型名字较长，将其改为"Employee Image Cropper"。

添加两个新的裁剪器，名为"Thumbnail" (200px x 350px) 和 "wideThumbnail" (350px x 200px)。

![定义裁剪器](images/Creating-Media-Crops-740.jpg)

剩下的四个属性分别命名为："Width", "Height", "Size" 和 "Type"，同时如下图一样修改他们的别名。它们全都使用 __Label__ 编辑器。如前所述，这些属性将在图像上传后自动填充。

![添加属性](images/Creating-Media-Properties-740.jpg)

### 定义媒体类型文件夹 ###
下一步我们创建一个目录容纳employee图片。我们可以使用已经存在的 __Folder__ 媒体类型，但是这也意味着编辑者可能将 employee 图片上传到其他任何地方。如果我们专门为employee图像创建一个文件夹，那么将只有一个地方存放它们，从而使媒体区块的组织变得更加容易。

#### 结构和继承 ####
返回 __设置__ 区块，创建一个新的媒体类型命名为"Employee Images"。通过点击名称左边的图标，选择文件夹图标。

我们希望它具备类似于 __Folder__ 一样的基本功能（例如属性、标签页），可以通过点击 __Compositions__ 然后选择 __Folder__ 媒体类型来实现。现在"Employee images"继承了*Foler*媒体类型的标签页和属性。

![组成](images/Creating-Media-Compositions.jpg)

最后我们需要设置允许employee图片创建在新目录中。前去 __Permissions__ 标签。点击 __Employee images__ 下面的  __Add child__ 。

![权限](images/Creating-Media-Permissions.jpg)

剩下要做的就是定义文件夹可以在哪里被创建。我们想在媒体区块的根下面创建文件夹，因此勾选 *Permission* 标签中 __Allow at root__ 选项来设置。

#### 创建文件夹和媒体条目 ####

前去 __Media__ 区块，点击媒体节点的菜单图标，接下来选择 __Employee images__ 文件夹。命名为 "Employee Images" 点击创建。

开始上传文件到目录，可以通过点击 __Employee images__ 节点上的菜单图标或者在内容视图中使用 __Create__ 按钮，然后选择 __Employee image__ 进行上传。

![上传媒体](images/Creating-Media-Upload-740.jpg)

*记住你可以取消 __Employee images__ 媒体类型的 __Allow at root__ 属性来防止编辑者创建多个该类型的目录。这只会禁止创建一个新，而不会影响到已经创建的目录*

#### 裁剪图片 ####
![裁剪图片](images/Creating-Media-Cropping-740.jpg)

如果你选择了一张图片并将它们上传到了目录中，你会看到完整的图片和两个我们之前定义了的裁剪图片。在图片上移动蓝色的焦点，会根据焦点区域更新裁剪图。你还可以通过选择并移动图像或者调整滑块缩放来编辑单个裁剪图。

###更多信息
- [输出媒体](../../Design/Rendering-Media/)
- [个性化数据类型](../Data-Types/index.md)

###相关服务
- [媒体服务](../../../Reference/Management/Services/MediaService.md)