# 教程 - 创建自定义仪表板 #

## 概述 ##
本指南提供了在 Umbraco配置一个简单的自定义仪表板的步骤。

### 什么是仪表板? ###

仪表板是指板块右边的某个标签页。例如："Content"板块的"Redirect Url Management"仪表板。
![Redirect Url Management Dashboard](images/whatisadashboard.jpg)

### 为什么给你的编辑提供一个自定义仪表板？ ###

一般来说当您构建一个 Umbraco 网站时，提供一个自定义仪表板用来欢迎您的编辑者，同时提供一些网站的相关信息以及提供一些编辑者可以使用的常用功能入口，会是很好的习惯做法。本指南会展示如何创建一个自定义的'欢迎信息'仪表板的基本知识，然后说明如何进一步使用AngularJS提供交互式体验……

所有我们要进行的操作步骤如下:

- 安装一个控制台插件
- 编写基本的欢迎信息视图
- 配置自定义欢迎控制台并显示
- 添加样式
- 添加 AngularJS 控制器
- 在欢迎信息中显示当前用户的名字
- 显示当前用户的最近更新的内容
- 创建一个快捷按钮用来发布新的博客
- 你可以做任何事情...


### 先决条件 ###
本指南仅介绍了在Umbraco中使用AngularJS，因此并不包含 AngularJS 本身的内容，我们已经为您准备了一些相关资源：

- [egghead.io](https://egghead.io/courses/angularjs-fundamentals)
- [angularjs.org/tutorial](https://docs.angularjs.org/tutorial)
- [Pluralsight](https://www.pluralsight.com/paths/angular-js)

这部分内容与创建属性编辑器有很多相似之处，教程'[创建一个属性编辑器教程](../../Tutorials/creating-a-property-editor/index.md)'也非常值得一读。

### 最终结果 ###
当本指南结束后，我们会拥有一个友好的欢迎仪表板来显示当前用户最近更新网站内容的列表。

## 安装一个插件 ##
首先我们必须在网站的`/App_Plugins`文件夹中创建一个新的文件夹。我们命名为`CustomWelcomeDashboard `。

## 创建控制台视图 ##
接下来我们在这个文件中创建一个 HTML 文件，命名为'WelcomeDashboard.html'，这个 html 文件包含一些 html 文档片段，不需要&lt;html&gt;&lt;head&gt;&lt;body&gt;这些元素。

添加下面的 html 到WelcomeDashboard.html

    <div class="welcome-dashboard">
        <h1>Welcome to Umbraco</h1>
        <p>We hope you find the experience of editing your content with Umbraco enjoyable and delightful. If you discover any problems with the site please report them to the support team at <a href="mailto:">support@popularumbracopartner.com</a></p>
        <p>You can put anything here...</p>
    </div>

## 配置仪表板的显示 ##

从你网站的 /config 文件夹中，打开dashboard.config文件[控制台配置解释在这里...](../../Extending/Dashboards/index.md)。

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

虽然这里的术语会有些混乱，但是我们确实创建了一个"Section"（但这不同于'Content Section'的 section - 在这个配置文件中称之为"Area"），这是一个特殊的"仪表板Area"，你可以用来将控制台分组并且一起控制它们。

上面的配置事实上说的是：

> "为Umbraco 网站的'Content' 区域/区块添加一个名为'Welcome'的标签页，使用WelcomeDashboard.html文件作为控制台的视图，同时不允许'translators'看到它！ "

*__注意:__ 标签页在 Umbraco 后台中的顺序，取决于它在 dashboard.config 文件中的位置，因此想要我们的自定义欢迎消息是编辑者在 Content 板块中默认看到的第一个，确保上面的配置在dashboard.config文件中是'最前面的'区块配置。*

*__注意:__ 您可以指定要在特定选项卡上显示的多个控件，以及特定部分中的多个选项卡。*

*__注意:__ 你可以删除已有的仪表板，以及通过更改Dashboard.config文件中的其他配置来控制谁能看到它们。*

### 结果 ###

![Custom Dashboard Welcome Message](images/welcomemessage.jpg)

## 添加一些样式 ##

恭喜你！完成了 - 但这并不是真正意义的完成，这只是一个起点。你需要的是带有样式的，接下来还有几步操作，才能将自定义的样式表表添加到控制台中：

我们需要在CustomWelcomeDashboard文件夹中创建package.manifest并写入一些东西

*__注意:__ 这个文件允许 Umbraco 载入一些其他资源供你的 HTML 视图使用 - 按照约定名为'package.manifest'，包含资源的配置信息，通过JSON 格式加载*

当 Umbraco 加载仪表板时，会在和 HTML 视图相同的目录中查找这个文件（记住控制台配置指向的 html 视图），并且使用清单载入外部的资源，例如 CSS 和 JS 文件。

这个清单文件非常类似于你前面创建[属性编辑器](../../Extending/Property-Editors/package-manifest.md)时的文件。

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


现在我们在CustomWelcomeDashboard文件夹中创建'customwelcomedashboard.css'，再添加一些样式：

	.welcome-dashboard h1 {
		font-size:4em;
		color:purple;
	}

这个样式现在会被加载并且应用于你的仪表板。根据需要，再添加一些图片和 html 标签：

![Custom Dashboard Welcome Message With styles...](images/welcomemessagewithstyles.jpg)

*__注意:__ 在 Umbraco 启动时，会将清单文件加载到内存中，因此如果你添加了新的样式表和脚本文件，你必须重启你的应用来加载它们。*

如果标题没有改变颜色，可能是因为在非调试模式下运行网站，[ClientDependency Framework](https://github.com/Shazwazza/ClientDependency) (CDF)会缓存 CSS 和 JavaScript。清除 CDF 缓存并获取新的 JavaScript 和 CSS，您需要增加[ClientDependency.config 文件](https://github.com/Shazwazza/ClientDependency/wiki/Configuration#complete-config)的版本号并保存。现在当你重新加载页面时，会看到有颜色的标题。

希望您现在可以看到，当用户登录进入 Umbraco时，将会看到您为他们提供的基本的欢迎仪表板。

## 添加功能 ##

我们可以通过 AngularJS 控制器和 HTML 视图来给控制台增加一些功能。

在CustomWelcomeDashboard文件夹中，创建一个新文件'customwelcomedashboard.controller.js'，控制器代码就写在里面。

我们把这个 AngularJS 控制器注册到 Umbraco Angular 模块：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope) {
        var vm = this;
        alert('hello world');
    });

在 html 视图视图中，更改外层 div来给视图添加一个控制器：

	<div class="welcome-dashboard" ng-controller="CustomWelcomeDashboardController as vm">

*__注意:__ 使用 vm（view model 的缩写）可以在视图和控制器之间通讯*

最后我们要更新包装清单文件package.manifest，在控制台显示时，加载外部的控制器文件：

	{
		"javascript":[
			/* 所有 js 文件用逗号分隔放在这里： */
			"~/app_plugins/CustomWelcomeDashboard/customwelcomedashboard.controller.js"
		],
		"css": [
			/* 所有 css 文件用逗号分隔放在这里: */
			"~/app_plugins/CustomWelcomeDashboard/customwelcomedashboard.css"
		]
	}


如果一切配置都是正确的，我们现在每次在 Umbraco 中加载'Content'板块的仪表板时，都会收到"Hello World"提示，然而这并没有什么实际意义。


### 进一步深入 - Umbraco Angular 服务和指令 ###

Umbraco 提供了很多 angular 指令，资源和服务，你也可以在自定义属性编辑器和控制台中使用它们，具体信息可以在这里查看：[https://our.umbraco.org/apidocs/ui/#/api](https://our.umbraco.org/apidocs/ui/#/api)


对于本例，显示出当前用户的名字是比较友好的（对于每个人 Umbraco 都在同一位置显示名字……），为了实现这一点，我们在这里可以使用**userService（注意这个服务在7.12.4版本中会报错，官方反馈在7.13版本中修复了这个问题）**来定制仪表板欢迎信息，已增加友好性：

我们把**userService**注入到AAngularJS控制器中，代码如下：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope,userService) {

然后我们使用userService的原生方法**getCurrentUser()**来获取当前登录用户的详细信息：

	var user = userService.getCurrentUser().then(function (user) {
		console.log(user);
		vm.UserName = user.name;
	});

*__提示:__ 注意你可以使用 console.log 把返回信息输出到浏览器的控制台窗口，这可以帮助我们进行调试，还可以用来了解可用的属性。*

最终我们更改视图中的代码，在当前登录用户的名字，显示在欢迎信息中：

	<h1>Welcome {{vm.UserName}} ...to Umbraco</h1>

![Custom Dashboard Welcome Message With Current User's Name](images/welcomemessagepersonalised.jpg)

作为参考，本指南中所涉及的*customwelcomedashboard.controller.js*文件的完整内容，看起来应该是下面这个样子的：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope,userService) {
		var vm = this;
		vm.UserName = 'guest';
		
		var user = userService.getCurrentUser().then(function (user) {
			console.log(user);
			vm.UserName = user.name;
		});
	});

## 我知道你上星期二做了什么... ##

当用户再次返回时，或许让他们看到最近编辑的几篇文章，并提供快捷链接可以继续编辑会是很有用的做法（而不是必须记住这些内容，并从 Umbraco 的内容树中再次找到它们）。

我们可以使用 Umbraco 提供的Angular资源来检索日志信息，**logResource**使用**getUserLog**方法来返回用户最近保存过的条目列表。

我们把logResource注入到控制器中：

	angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope, userService, logResource) {

添加属性到 ViewModel 对象，用于存储日志信息：

	vm.UserLogHistory = [];

添加angular 提供的*ng-repeat*标签到我们的WelcomeDashboard.html视图中，用来循环输出日志条目：

	<h2>We know what you edited last week...</h2>
	<ul>
		<li ng-repeat="logEntry in vm.UserLogHistory.items">{{logEntry.nodeId}} - {{logEntry.comment}} - {{logEntry.timestamp  | date:'medium'}}</li>
	</ul>

返回控制器，我们使用**logResource**来填充数组，getPagedUserLog方法要接收一个 JSON 对象信息，用来筛选日志：

	 var userLogOptions = {
        pageSize: 10,
        pageNumber: 1,
        orderDirection: "Descending",
        sinceDate: new Date(2018, 0, 1)
    };

这些配置项会检索当前用户从2018年开始，按照倒序排列的最后10条信息，我们把选项传递给 **getPagedUserLog**：

    logResource.getPagedUserLog(userLogOptions)
        .then(function (response) {
            console.log(response)
            vm.UserLogHistory = response;
        });

在你的浏览器中可以查看到console.log 输出的从日志中检索到的响应信息：

    {pageNumber: 2, pageSize: 10, totalPages: 6, totalItems: 60, items: Array(10)}
        items: Array(10)
            0:
                $$hashKey: "03L"
                comment: "Save and Publish performed by user"
                logType: "Publish"
                nodeId: 1101
                timestamp: "2018-11-25T13:40:11.137Z"
                userAvatars: (5) ["https://www.gravatar.com/avatar/1da605eb2601035122149d0bc1edb5ea?d=404&s=30", "https://www.gravatar.com/avatar/1da605eb2601035122149d0bc1edb5ea?d=404&s=60", "https://www.gravatar.com/avatar/1da605eb2601035122149d0bc1edb5ea?d=404&s=90", "https://www.gravatar.com/avatar/1da605eb2601035122149d0bc1edb5ea?d=404&s=150", "https://www.gravatar.com/avatar/1da605eb2601035122149d0bc1edb5ea?d=404&s=300"]
                userId: 0
                userName: "marc"


这几乎就是我们所需要的了，但是还缺少有关保存和发布的信息条目！

我们可以使用**entityResource**，Umbraco 提供的另外一个Argular 资源，可以检索到更多关于给定 ID 的实体信息。

把它注入到 Angular 控制器：

    angular.module("umbraco").controller("CustomWelcomeDashboardController", function ($scope, userService, logResource, entityResource) {

我们需要从**logResource**中遍历日志项，因为其中包含所有的内容，因此我们需要筛选出我们不需要的东西，例如：宏保存记录或 doumenttype 保存，一般来说我们需要的只是日志条目中包含 nodeId、'logType'为'save'的媒体或内容。

**entityResource**提供一个**getById**方法，根据传入的条目的 Id 以及实体类别来检索这个实体的有用信息，比如它的名字和图标。

合并代码如下:

	logResource.getPagedUserLog(userLogoptions).then(function (response) {
		console.log(response);
		vm.UserLogHistory = response;
		var filteredLogEntries = [];
		//从响应中遍历信息，并过滤掉我们不感兴趣的保存类日志
		angular.forEach(response.items, function (item) {
			// 如果不存在实体对象，nodeId 会返回-1（例如保存宏就没有 nodeid）
			if (item.nodeId > 0) {
				if (item.logType == "Save") {
					if (item.entityType == "Media") {
						// 日志实体是媒体条目
						item.editUrl = "media/media/edit/" + item.nodeId;
					}
					if (item.entityType == "Document") {
						// 日志实体是内容条目
						item.editUrl = "content/content/edit/" + item.nodeId;
					}
					//使用entityResource来检索内容或媒体条目的详细信息
					entityResource.getById(item.nodeId, item.entityType).then(function (ent) {
						console.log(ent);
						item.Content = ent;
					});
					filteredLogEntries.push(item);
				}
			}
		});
		vm.UserLogHistory.items = filteredLogEntries;
	});

最后更新我们的视图来使用这些检索到的实体信息：

	<h2>We know what you edited last week...</h2>
	<ul class="unstyled">
		<li ng-repeat="logEntry in vm.UserLogHistory.items"><i class="{{logEntry.Content.icon}}"></i> <a href="/Umbraco/#/{{logEntry.editUrl}}">{{logEntry.Content.name}}</a> - <span class="text-muted">(Edited on: {{logEntry.timestamp  | date:'medium'}})</span></li>
	</ul>
	
现在我们就可以看到最近保存的内容和媒体列表了：

![We know what you edited last week...](images/WeKnowWhatYouEditedLastWeek.jpg)

*__注意:__  url"/Umbraco/#/content/content/edit/1234"是打开特定实体（id 为1234的）用来编辑的路径信息。*

*__注意:__ 不幸的是logResource有一些断崖式的变化，（在使用 SQLCE 数据库时），早期版本到7.6.4该资源会报告404；从7.6.4-7.13你可以使用logResource.getUserLog("save", new Date()).then(function (response);7.13之后，你可以使用getPagedUserLog方法来替代，如上所示，这在 SQLCE 中也是可以正常工作的*

## 我知道你今天想要做什么 ##
一个关键用户journeys会进入后台创建一些算得上是新的内容，如果一个人的工作是一天中，在同一板块多次创建博客条目，为什么不创建一些方便的快捷方式来完成这些常见任务：

我们可以约定一个链接'编辑页面'(如上所述)，通过传递扩展的查询参数 `doctype=alias` 和`create=true`前去指定页面，便于当前用户在指定的板块根据别名(alias)来创建新的内容。

添加下面的内容到视图：

	<div>
		<a class="btn btn-primary btn-large" href="/umbraco/#/content/content/edit/1075?doctype=BlogPost&create=true"><i class="icon-edit"></i>Create New Blog Post</a>
	</div>

1075是博客板块的 id，BlogPost是我们想要创建的文档的别名。

![Handy shortcut buttons](images/CreateNewBlogPost.jpg)

## 自定义外部数据 - 创建你自己的 Angular 资源 ##

你可以创建自定义的 angular 服务/资源，来整合你的服务端数据（使用UmbracoAuthorizedJsonController）。属性编辑器指南中的一个步骤，已经描述了该如何去做[第四部分 - 添加服务端数据给属性编辑器](../../Tutorials/creating-a-property-editor/part-4.md).




## 还有什么? - 你还在等什么? ##
仪表板或许是第一个第三方系统的接口，或者用于搜索指定内容的工具，或者清除已有内容的工具。等等等等……。

![really you can put anything here](images/asteroids.jpg)


