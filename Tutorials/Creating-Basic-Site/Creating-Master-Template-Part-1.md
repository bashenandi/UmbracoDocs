# 创建更多页面 #

## 使用可维护的模板结构 ##

我们再看一下**_Document Type_**是如何创建的。如果我们想创建一个包含三个简单页面的网站；首页，新闻和联系我们页面。我们只要简单地为这些页面创建带模板的**_Document Type_**，复制相同的 HTML 代码到每个模板中，再在每个模板中定制不同的修改。

这样是可以运作的（在非常简单的网站上，确实有一些优点），然而一旦你的网站开始快速成长就会出现问题 - 例如要更改任何头部和底部的信息，你都改修改每个模板 - 在这个例子中意味着我们要在每个页面中输入同样的底部内容。

对于基本一致的模板，Umbraco 为我们提供了一个优雅的解决方案 - 熟悉 MVC 的人会认出它。

开始做这些我们要拆解已经创建好的 homepage，使 homepage 位于 master 之下。

## 创建主模板 ##

前去**_Settings > Templates_**展开树。此时我们只有**_Homepage_**模板。鼠标移动到**_Templates_**菜单上点击**_..._** 按钮，创建一个叫 Master 的新模板。点击**_+ Create_**后给它一个名字叫"**_Master_**"，别忘了点**_Save_**。


![主模板](images/figure-22-master-template.png)

*Figure 22 - 主模板*

现在我们去把**_Homepage_**模板移动到**_Master_**模板下面。要做这些我们打开**_Settings > Templates > Homepage_**节点，在**_Properties 标签页 > Master template 下拉列表_**中选择_"Master"_之后点击**_Save_**。实际上这会更新 Razor 代码中的`Layout = null;`变为`Layout = "Master.cshtml";`。

>注意: 在旧版本的 Umbraco 中，这里可能会遇到一个 bug，需要你先点击**_Homepage_**节点之后，再返回来才能看到这些更改。



![Homepage 模板现在位于主模板之下](images/figure-23-homepage-has-master-template.png)
*Figure 23 - Homepage 模板现在位于主模板下方*


For this site, we'll assume this is part of the child page. In the template edit screen cut everything from the closing curly brace to line 37 `<div id="main-container">` - we're going to move the header and nav of the site to the master template. Cut this and click **_Save_**. 

现在我们需要把所有页面都需要的，模板中通用的部分移动到**_Master_**中。做为开发人员，这里你可能需要动动脑子，因为每个网站都会有所不同 - 例如所有网站都包含`<div id="main">`部分，那我们是把它放在主模板中还是它只属于某些页面？对于本站我们假设这是子页面中的一部分。在模板编辑界面中，剪切从关闭的花括号到37行`<div id="main-container">` - 我们移动网站的头部和导航到主模板中。剪切这些代码然后点击**_Save_**。


![去掉头部之后的Homepage模板](images/figure-24-homepage-after-cutting-the-header.png)
*Figure 24 - 去掉头部之后的Homepage模板*


现在点击的 **_Master_** 模板，粘贴前面复制的 HTML 标记到关闭花括号的后面，记得点击**_Save_**。

![粘贴头部之后的 Master 模板](images/figure-25-master-template-with-header.png)
*Figure 25 - 粘贴头部之后的 Master 模板*

在这些标记的后面我们需要告诉 Umbraco 插入的子模板中的内容 - 这需要添加代码**_@RenderBody()_**到结尾处（37行附近）。点击**_Save_**。

![把RenderBody()添加到 Master 模板](images/figure-26-adding-renderbody.png)
*Figure 26 - 把RenderBody()添加到 Master 模板*

底部内容我们进行同样的操作。打开**_Settings > Templates > Homepage > template_**标签页，从**_footer-container _** div（大约在35行）的开始标记之前，剪切所有内容，点击**_Save_**保存，然后把这些代码粘贴到**_Master_**中我们刚才添加的**_@RenderBody_**段落后面。记得点击**_Save_**。

![完整的主模板](images/figure-27-master-template-complete.png)
*Figure 27 - 完整的主模板*

截止当前我们我们已经做了很多工作 - 然而我们刷新本地页面时并不会看到任何改变！如果你遇到了编译错误，你可能忘记了**_@RenderBody()_**。如果你丢失了任何内容（头部或底部），检查它们让你的模板看起来和下面一样：

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
	    Layout = null;
	}
	<!doctype html>
	<!--[if lt IE 7]> <html class="no-js ie6 oldie" lang="en"> <![endif]-->
	<!--[if IE 7]>    <html class="no-js ie7 oldie" lang="en"> <![endif]-->
	<!--[if IE 8]>    <html class="no-js ie8 oldie" lang="en"> <![endif]-->
	<!--[if gt IE 8]><!--> <html class="no-js" lang="en"> <!--<![endif]-->
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	
		<title></title>
		<meta name="description" content="">
		<meta name="author" content="">
	
		<meta name="viewport" content="width=device-width,initial-scale=1">
	
		<link rel="stylesheet" href="css/style.css">
	
		<script src="js/libs/modernizr-2.0.6.min.js"></script>
	</head>
	<body>
	
		<div id="header-container">
			<header class="wrapper clearfix">
				<h1 id="title">@Umbraco.Field("pageTitle")</h1>
				<nav>
					<ul>
						<li><a href="#">nav ul li a</a></li>
						<li><a href="#">nav ul li a</a></li>
						<li><a href="#">nav ul li a</a></li>
					</ul>
				</nav>
			</header>
		</div>
				
		@RenderBody()
				
		<div id="footer-container">
			<footer class="wrapper">
				<h3>@Umbraco.Field("footerText")</h3>
			</footer>
		</div>
	
	</body>
	</html>

*Figure 28 - 完整的主模板*

	@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
	@{
	    Layout = "Master.cshtml";
	}
	<div id="main-container">
		<div id="main" class="wrapper clearfix">
			
			<article>
				<header>
					@Umbraco.Field("bodyText")
				</header>
				<section>
					<h2>article section h2</h2>
					<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam sodales urna non odio egestas tempor. Nunc vel vehicula ante. Etiam bibendum iaculis libero, eget molestie nisl pharetra in. In semper consequat est, eu porta velit mollis nec. Curabitur posuere enim eget turpis feugiat tempor. Etiam ullamcorper lorem dapibus velit suscipit ultrices. Proin in est sed erat facilisis pharetra.</p>
				</section>
				<section>
					<h2>article section h2</h2>
					<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam sodales urna non odio egestas tempor. Nunc vel vehicula ante. Etiam bibendum iaculis libero, eget molestie nisl pharetra in. In semper consequat est, eu porta velit mollis nec. Curabitur posuere enim eget turpis feugiat tempor. Etiam ullamcorper lorem dapibus velit suscipit ultrices. Proin in est sed erat facilisis pharetra.</p>
				</section>
				<footer>
					<h3>article footer h3</h3>
					<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam sodales urna non odio egestas tempor. Nunc vel vehicula ante. Etiam bibendum iaculis libero, eget molestie nisl pharetra in. In semper consequat est, eu porta velit mollis nec. Curabitur posuere enim eget turpis feugiat tempor.</p>
				</footer>
			</article>
			
			<aside>
				<h3>aside</h3>
				<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aliquam sodales urna non odio egestas tempor. Nunc vel vehicula ante. Etiam bibendum iaculis libero, eget molestie nisl pharetra in. In semper consequat est, eu porta velit mollis nec. Curabitur posuere enim eget turpis feugiat tempor. Etiam ullamcorper lorem dapibus velit suscipit ultrices.</p>
			</aside>
			
		</div> <!-- #main -->
	</div> <!-- #main-container -->

*Figure 29 - 完整的 Homepage 模板*


> 如果你对这些概念还是陌生的，我不认为在开始下一步之前做其他事是有意义的。还请反复阅读，操作这一章内容。


## 下一步 - [创建主模板 - 第二部分](Creating-Master-Template-Part-2.md) ##
第二部分 - 使用主模板创建新的页面类型。