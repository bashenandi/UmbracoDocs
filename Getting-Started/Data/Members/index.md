# 成员 #
成员用于网站注册以及一些外部扩展安装包（例如：论坛用户，内网用户等等）。

不同于文档类型和媒体类型，会员的定义、创建和编辑都是在**Member**板块中完成的。本指南讲述了如何在后台定义和创建用户。如果你想通过服务 APIs 来操作会员，在文档底部可以找到链接。

系统提供了默认的成员类型用于创建成员。当然，您可以根据需要对其进行自定义，或者从头开始创建自己的成员类型。

## 创建会员 ##
前往 __Members__ 区块，点击会员树中 __Members__ 的菜单按钮之后选择 __Member__ 。会员有一些必填的属性。所以除了名字之外你还需要在 __Properties__ 标签页之中填写： __Login__, __Email__ and __Password__ 才可以保存。

在 __Membership__ 标签页中还有一些默认的属性：

- umbracoMemberComments
- umbracoMemberFailedPasswordAttempts
- umbracoMemberApproved
- umbracoMemberLockedOut
- umbracoMemberLastLockoutDate
- umbracoMemberLastLogin
- umbracoMemberLastPasswordChangeDate

一旦成员创建并保存后，你可以通过展开会员树中的 __All Members__ 获取列表视图（可以实时搜索），或者在会员树中通过选择会员类型进行过滤。

## 创建会员类型 ##
你可以创建你自己的会员类型，并且添加标签页和属性，类似于文档类型。

前往 __Members__ 区块，点击会员树中 __Member Types__ 的菜单按钮之后选择 __Create__ 。命名新的会员类型之后点击 __Create__ 按钮。

现在你会看到会员类型编辑器用来定义和编辑会员类型。它包含三个标签页： __Info__, __Generic Properties__ 和 __Tabs__ 。

### 信息 ###
![会员信息页](images/Members-Info.jpg)

显示关于会员类型的基础信息和自定义属性设置。

__Name:__ 会员类型的名字会显示在会员树中和用户创建新的会员时。

__Alias:__ 用于在代码中引用会员类型。

__Icon:__ 显示在会员列表视图中的图标。如果有超过一个的会员类型都选择不同的图标，会有利于你简便的识别会员。

__Description:__ 描述显示在创建会员的时候。

__Property settings:__ 如果会员类型添加了属性，你可以在前端控制操作这些属性：

  - __Member can edit:__ 登录成员可以编辑这个属性。
  - __Show on profile:__ 成员登录后可以显示在个人资料中。

### 通用属性 ###
![通用属性标签](images/Members-Generic-Properties.jpg)
为媒体类型创建、编辑和组织属性。

##### 添加属性 #####
想要给会员类型添加属性选择  __Click here to add new property__ 。

__Name:__ 属性名称。

__Alias:__ 在模板中引用这个属性。

__Type:__ 选择的类型将决定此属性的输入方法。例如 *Richtext editor*, *Date Picker*, *Image Cropper* 等等。你可以在 __Developer Section__ 下面的 __Data Type__ 创建或编辑新的类型。

__Tab:__ 属性显示的标签页。额外的标签页可以在 __Tabs__ 标签页中创建。如果这个属性被设置为*Generic Properties*，那么它在内容视图中会被显示在*Properties*标签页中。

__Mandatory:__ 要求属性必填，意味着如果这个属性没有值，是不能创建并保存的。

__Validation:__ 添加一个正则表达式，在保存是验证这个属性。

__Description:__ 当创建/编辑会员时，描述会显示在属性名字的下方。好的描述对编辑者会有很重要的帮助。

#####组织属性
通过拖放来组织管理属性。如果有多个标签页，可以在不同的标签页之间拖放属性。

### 标签
![会员标签标签页](images/Members-Tabs.jpg)
通过在输入框中输入名字然后点击  __New tab__ 按钮来创建一个新的标签页。

##### 名字和排序
重名一个标签页就像文档类型一样，在输入框中更改名字就可以完成。改变标签页的排序可以通过拖拽左侧或者在第二个输入框中输入数字。在内容区块中，标签从左（低的数值）向右（高的数值）显示。

##创建会员组
成员组可以为你的成员定义角色，从而实现基于角色的保护。一个成员可以属于不同的组。

![创建会员组](images/Member-Groups-Create.jpg)

要创建一个新的会员组，在会员区块中点击  __Member Groups__ 节点，选择 __Create__ ，命名并且保存组。

###分配会员组
要分配会员到指定的会员组，先找到想要分配的会员，前往 __Properties__ 标签。在 *Member Group* 属性下面，有两列：

![指派会员组](images/Member-Groups-Assign.jpg)

__NOT A MEMBER OF GROUP(S):__ 列出所有不包含该会员的组。指派会员到一个组，只需要点击它，就会移动到另一列。

__MEMBER OF GROUP(S):__ 列出所有包含该会员的组。从某个组里删除会员，只要点击该组，就会移动到另一列。

#技巧
作为一个开发者，基于 Umbraco 的会员区块进行构建时，可以使你的网站更有优势。

虽然会员板块默认是在 Umbraco 的后台。你应该做些工作在你的网站前端实现它。会员由 ASP.NET 提供的membership自定义而成。会员组由自定义的角色提供。它们都在 web.config 中定义。你可以通过下面的链接，在文档的参考部分，找到更多相关的服务方法。

###更多信息
- [自定义数据类型](../Data-Types/)

###相关服务
- [会员服务](../../../Reference/Management/Services/MemberService.md)
- [会员类型服务](../../../Reference/Management/Services/MemberTypeService.md)
- [会员组服务](../../../Reference/Management/Services/MemberGroupService.md)

### [Umbraco.TV](https://umbraco.tv)
- [Chapter: Members](https://umbraco.tv/videos/umbraco-v7/content-editor/administrative-content/members/what-is-a-member/)
- Member API chapter *(Coming soon)*