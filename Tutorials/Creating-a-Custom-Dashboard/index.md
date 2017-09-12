# 教程 - 创建自定义控制台

## 概述
这篇教程让你一步一步在 Umbraco 中完成一个简单的自定义控制台。

### 什么是控制台?

区块右手边的标签，例如：内容区块的"Redirect Url Management"控制台：

![Redirect Url Management Dashboard](images/whatisadashboard.jpg)

### 为什么?
当你搭建一个 Umbraco 站点时，提供一个自定义的控制台用于欢迎编辑者、提供关于网站的信息、提供一些有帮助的通用功能会被认为是比较好的做法。本指导会教你创建一个自定义'Welcome Message'控制台的基础，以及如何使用 Angular 提供更多的功能...

所有我们要进行的步骤:

- 安装一个控制台插件
- 编写基本的欢迎信息视图
- 配置自定义欢迎控制台并显示
- 添加样式
- 添加 AngularJS 控制器
- 在欢迎信息中显示当前用户的名字
- 显示当前用户的最近更新
- 创建一个快捷键用于添加博客回复
- 你可以做任何事情...

### 先决条件
这是关于如何在 Umbraco 中使用 AngularJS，因此并不包含 AngularJS 自身的内容，这里已经为你准备了一些资源：

- [egghead.io](http://www.egghead.io/)
- [angularjs.org/tutorial](http://docs.angularjs.org/tutorial)
- [Tekpub](http://tekpub.com/products/angular)

这里与创建属性编辑器比较类似，教程'[创建一个属性编辑器教程](../../Tutorials/creating-a-property-editor/index.md)'。

### 最终结果
在教程的结尾，我们会有一个友好的欢迎控制台，可以显示当前用户最近操作网站的记录列表。

## 安装一个插件
第一件事我们必须在网站的'/App_Plugins'文件夹中创建一个新的文件夹。我们叫它'CustomWelcomeDashboard'。

## 创建控制台视图
接下来我们在这个文件中创建一个 HTML 文件，命名为'WelcomeDashboard.html'，这个 html 文件包含一些 html 文档片段，不需要&lt;html&gt;&lt;head&gt;&lt;body&gt;。

添加下面的 html 到WelcomeDashboard.html

    <div class="welcome-dashboard">
        <h1>Welcome to Umbraco</h1>
        <p>We hope you find the experience of editing your content with Umbraco enjoyable and delightful. If you discover any problems with the site please report them to the support team at <a href="mailto:">support@popularumbracopartner.com</a></p>
        <p>You can put anything here...</p>
    </div>

## 配置控制台的呈现
从你网站的/config文件夹中打开dashboard.config文件[控制台配置解释在这里...](../../Extending/Dashboards/index.md)。

添加下面的部分：

    <section alias="Custom Welcome Dashboard">
        <access>
            <deny>translator</deny>
        </access>
        <areas>
            <area>content</area>
        </areas>
        <tab caption="Welcome">
            <control>
                /app_plugins/CustomWelcomeDashboard/WelcomeDashboard.html
            </control>
        </tab>
    </section>

这里的术语会有些混乱，但是我们已经创建了一个"Section"（但是这不同于'Content Section'的 section - 这在这个配置文件中引用为"Area"），这是一个特殊的"仪表板区块"，你可以用来将控制台分组和控制显示。

上面的配置事实上说的是：

> "添加一个名为'Welcome'的标签到 Umbraco 网站的'Content' 区域/区块，使用WelcomeDashboard.html文件作为控制台的视图，同时不允许'translators'看到它！ "

*__注意:__ 标签在 Umbraco 后台中的顺序，依赖于它在 dashboard.config 文件中的位置，因此想要我们的自定义欢迎消息是编辑者在 Content 区块中默认看到的第一个，确保上面的配置是dashboard.config文件中的'第一个'区块配置。*

*__注意:__ 你可以指定多个控制视图呈现在一个标签中，和多个标签在同一个区块中。*

*__注意:__ 你可以删除已有的控制台，通过更新Dashboard.config文件中的其他配置区块来控制谁能看到它们。*

### 结果

![Custom Dashboard Welcome Message](images/welcomemessage.jpg)

## 添加一些样式

恭喜你！完成了 - 但这并不是真正意义的完成，这只是控制台的起点，你需要的是带有样式的，接下来还有几步操作，才能将自定义的样式表表添加到控制台中：

我们需要在CustomWelcomeDashboard文件中创建package.manifest并写入一些东西

*__注意:__ 这个文件允许 Umbraco 载入一些其他资源供你的 HTML 视图使用 - 按照约定名为'package.manifest'，包含资源的配置信息，通过JSON 格式加载*

当 Umbraco 载入控制台时，会在和 HTML 视图相同的目录中查找这个文件（记住控制台配置指向的 html 视图），并且使用清单载入附加的资源，例如 CSS 和 JS 文件。

这个清单文件非常类似于你要创建[属性编辑器](../../Extending/Property-Editors/package-manifest.md)的文件。

在包装清单中加入一些 JSON 来描述控制台需要的脚本和样式表资源：

	{
		"javascript":[
			/*脚本文件列表在这里*/
		],
		"css": [
			/* 样式表文件在这里 */
			"~/app_plugins/CustomWelcomeDashboard/customwelcomedashboard.css"
		]
	}

现在在CustomWelcomeDashboard文件夹中创建'customwelcomedashboard.css'，再添加一些样式：

	.welcome-dashboard h1 {
		font-size:4em;
		color:purple;
	}

这个样式表现在会被加载并且应用于你的控制台。再添加一些需要的图片和 html 标记：

![Custom Dashboard Welcome Message With styles...](images/welcomemessagewithstyles.jpg)

*__注意:__ 在 Umbraco 启动时，会将清单文件加载到内存中，因此如果你添加了新的样式表和脚本文件，你必须重启你的应用来加载它们。*

如果一切正常，你会看到你提供给编辑者当他们登录到 Umbraco 看到的基础默认欢迎控制台。

## 添加功能

我们可以通过联合 AngularJS 控制器和 HTML 视图来给控制台增加一些功能。

让我们在CustomWelcomeDashboard文件夹中，创建一个新文件'customwelcomedashboard.controller.js'，控制器代码就在这里。

我们把这个 AngularJS 控制器注册到 Umbraco Angular 模块：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope) {
        var vm = this;
        alert('hello world');
    });

在 html 视图视图中，更改外层的 div为控制器：

	<div class="welcome-dashboard" ng-controller="CustomWelcomeDashboardController as vm">

*__注意:__ 使用 vm（view model 的缩写）可以在视图和控制器之间通讯*

最后我们要更新包装清单文件package.manifest，在控制台显示时，载入额外的控制器js 文件：

	{
		"javascript":[
			/*any comma delimited list of javascript files appear here*/
			"~/app_plugins/CustomWelcomeDashboard/customwelcomedashboard.controller.js"
		],
		"css": [
			/*a comma delimited list of stylesheets appear here:*/
			"~/app_plugins/CustomWelcomeDashboard/customwelcomedashboard.css"
		]
	}

如果这些都配置正确，当我们在 Umbraco 的内容区块再次打开控制台时，每次都会收到'Hello world'的提示信息，这很不友好。

### 进一步 - Umbraco Angular 服务和指令

Umbraco 有很多 angular 指令，资源和服务，你可以在你的自定义属性编辑器和控制台中使用它们，具体信息可以在这里查看：https://our.umbraco.org/apidocs/ui/#/api

在这个示例中，只是先友好的显示编辑者的名字（Umbrazo 有可以每个登陆者看到名字的地方……），为了实现这些我们需要使用**userService**，定制控制台欢迎信息来增加友好度：

我们像下面这个把**userService**注入到AAngularJS控制器：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope,userService) {

然后我们使用userService的方法**getCurrentUser()**来获取当前登录用户的详细信息：

	var user = userService.getCurrentUser().then(function (user) {
		console.log(user);
		vm.UserName = user.name;
	});

*__提示:__ 注意你可以使用 console.log 来输出信息到浏览器的控制台窗口，来了解返回信息，可以帮助调试，还可以用于了解用户的已有属性。*

最终再次更改视图中的代码，在欢迎消息中显示当前登录用户的名字：

	<h1>Welcome {{vm.UserName}} ...to Umbraco</h1>

![Custom Dashboard Welcome Message With Current User's Name](images/welcomemessagepersonalised.jpg)

最终这个教程中的customwelcomedashboard.controller.js文件，修改后的完整内容看起来应该是这样的：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope,userService) {
		var vm = this;
		vm.UserName = 'guest';
		
		var user = userService.getCurrentUser().then(function (user) {
			console.log(user);
			vm.UserName = user.name;
		});
	});

## 我知道你上星期二做了什么...

再次返回的编辑者可能想要友好的看到之前的的编辑信息，还带一个链接可以载入并且继续编辑（而不是必须记住，并且再从 Umbraco 内容树中找到它们）。

我们可以使用 Umbraco 的Angular资源来检索日志信息，**logResource**使用**getUserLog**方法来返回用户最近保存过的条目列表。

我们把logResource注入到控制器中：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope, userService, logResource) {

添加属性到 ViewModel 对象，用于存储日志信息：

	vm.LogEntries = [];

修改WelcomeDashboard.html添加一些标记，使用 angular 的*ng-repeat*来列表显示日志：

	<h2>We know what you edited last week...</h2>
	<ul>
		<li ng-repeat="logEntry in vm.LogEntries">{{logEntry.nodeId}} - {{logEntry.comment}} - {{logEntry.timestamp  | date:'medium'}}</li>
	</ul>

返回控制器中，使用**logResource**来填充实体数组，返回所有类型为'Save'的日志条目：

	logResource.getUserLog("save",new Date()).then(function (response){
		console.log(response);
		vm.LogEntries = response;
	});

现在我们查看控制台输出，可以看到**logResource**返回的数据内容，可能如下：

	 Object
        $$hashKey:"265"
        Content:
            Object
                comment:"Save Content performed by user"
                logType:1
                nodeId:1063
                timestamp:"2017-03-18T14:09:40.91"
                userId:0

这里通过一些少量的工作就完成了提供一些有意义的日志信息给到编辑者！

要从日志中向编辑者提供一些有意义的信息，还有一些事情要做！

我们能够使用**entityResource**，一个 Umbraco 的 Angular 资源，可以让我们根据实体的 id 来查询实体的更多信息。

先把它注入到控制器中：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope, userService, logResource, entityResource) {

我们要从**logResource**返回的响应中，过滤出我们不感兴趣的'saves'事件，例如：宏保存、文档类型保存，通常我们需要的是 nodeId以及注释文本中包含任何Media 和 Content的日志。

**entityResource**包含**getById**方法，接受条目的 Id 和实体的'类型'来检索出关于实体的有意义的信息，例如名称和图标。

代码如下:

	logResource.getUserLog("save", new Date()).then(function (response){
		console.log(response);
		var logEntries = [];
		/* 遍历响应，过滤出不感兴趣的日志 */
		angular.forEach(response, function (item) {
			/* 如果不是实体，返回的 nodeId 是-1（例如保存宏文件会创建一个不带 nodeid 的日志） */
			if (item.nodeId > 0) {
				/* 仅显示图片和内容的保存日志 */
				if (item.comment.match("(\\bContent\\b|\\bMedia\\b)")) {
					if (item.comment.indexOf("Media") > -1) {
						/* 日志实体是媒体条目 */
						item.entityType = "Media";
						item.editUrl = "media/media/edit/" + item.nodeId;
					}
					
					if (item.comment.indexOf("Content") > -1) {
						/* 日志实体是内容 */
						item.entityType = "Document";
						item.editUrl = "content/content/edit/" + item.nodeId;
					}
					
					/* 使用entityResource检索详细信息 */
					entityResource.getById(item.nodeId, item.entityType).then(function (ent) {
						console.log(ent);
						item.Content = ent;
					});
					logEntries.push(item);
				}
			}
			console.log(logEntries);
			vm.LogEntries = logEntries;
		});
 
最后更新视图使用格外的实体检索信息：

	<h2>We know what you edited last week...</h2>
	<ul class="unstyled">
		<li ng-repeat="logEntry in vm.LogEntries"><i class="{{logEntry.Content.icon}}"></i> <a href="/Umbraco/#/{{logEntry.editUrl}}">{{logEntry.Content.name}}</a> - <span class="text-muted">(Edited on: {{logEntry.timestamp  | date:'medium'}})</span></li>
	</ul>

现在我们就可以看到最近保存的内容和媒体的列表：

![We know what you edited last week...](images/WeKnowWhatYouEditedLastWeek.jpg)

*__注意:__  /Umbraco/#/content/content/edit/1234 是一个 url 路径，前去打开特定的（根据 id 1234）实体用于编辑。*

## 我知道今天你想要做什么
一个关键用户Journeys要在后台创建一个新的内容，如果这种工作需要每天在同一个板块进行多次，为什么不创建一个快捷按钮引导他们前去进行日常任务。

我们可以使用已有的知识，组装一个链接给到'edit a page'（或任意的按钮），通过附加的查询字符串 doctype=alias 和 create=true，可以使用户点击之后直接跳去指定板块根据别名来创建内容。

添加下面的内容到视图：

	<div>
		<a class="btn btn-primary btn-large" href="/umbraco/#/content/content/edit/1075?doctype=BlogPost&create=true"><i class="icon-edit"></i>Create New Blog Post</a>
	</div>

1075是博客板块的 id，BlogPost是我们想要创建的文档的别名。

![Handy short cut buttons](images/CreateNewBlogPost.jpg)

## 自定义外部数据 - 创建你自己的 Angular 资源

你可以创建自己的 angular 服务/资源，整合你自己的服务端数据（使用UmbracoAuthorizedJsonController）。在属性编辑器教程中，已经描述了该如何做这些[第四部分 - 添加服务端数据给属性编辑器](../../Tutorials/creating-a-property-editor/part-4.md).

## 还有什么? - 你还在等什么?
可能控制台是第三方系统的接口，或者是用于搜索指定内容的工具，或者帮助我们清理已有内容的工具。你可以根据子级的需要，不断的扩展、扩展再扩展。

![really you can put anything here](images/asteroids.jpg)


