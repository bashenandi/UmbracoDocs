# 创建 HTML 表单 #

_创建一个 HTML 表单用来提交数据到 Umbraco 是非常简单的！你只需要创建一个 SurfaceController，一个'View Model'类再使用一个方便的叫BeginUmbracoForm的HtmlHelper 扩展方法_

## 快速链接 - MVC 表单教程 ##

* [使用局部视图创建 MVC 表单](Forms/tutorial-partial-views.md)
* [使用子 Action 创建 MVC 表单](Forms/tutorial-child-action.md)
* [使用自定义html 标记创建 MVC 表单](Forms/tutorial-custom-markup.md)

## 创建表单 - View Model ##
首先我们需要定义要提交的数据，这通过创建一个'Voew Model'类来完成。这是一个示例：
	
	public class CommentViewModel
	{
	    [Required]
	    public string Name { get; set; }
	
	    [Required]
	    public string Email { get; set; }
	
	    [Required]
	    [Display(Name = "Enter a comment")]
	    public string Comment { get; set; }
	}

这个类定义的数据是要提交的数据，还定义了哪些数据是必填的，MVC 会自动实现这些验证属性，前端验证 JS 也会自动执行。

## 创建 SurfaceController Action ##
下一步，我们需要在SurfaceController中创建一个 Action用来接收我们提交的视图数据。这有一个示例（这是本地定义的控制器）：

	public class BlogPostSurfaceController : Umbraco.Web.Mvc.SurfaceController
	{
		[HttpPost]
		public ActionResult CreateComment(CommentViewModel model)
		{
		    //模型未验证，不会保存，但是会返回到当前的 Umbraco 页面
		    if (!ModelState.IsValid)
			{
				//可能你想添加些自定义数据在 ViewBag 中
		   		return CurrentUmbracoPage();
			}
		
		    //如果验证通过执行一些逻辑
		    //In this sample we keep it empty, but try setting a breakpoint to see what is posted here
		    // Perhaps you might want to store some data in TempData which will be available 
		    // in the View after the redirect below. An example might be to show a custom 'submit
		    // successful' message on the View, for example:
		    TempData.Add("CustomMessage", "Your form was successfully submitted at " + DateTime.Now);
		    
		    //跳转到当前页面并清空数据
		    return RedirectToCurrentUmbracoPage();
		
		    //或者跳转到指定页面
		    //return RedirectToUmbracoPage(12345)
		}
	}

## 使用 BeginUmbracoForm ##
最后我们需要输出 HTML 表单来保证它们提交到 surface 控制器。最简单的方法是创建一个局部视图用来输出你的表单，并且将模型数据声明为你ViewModel。这里有一些重写的BeginUmbracoForm方法，我们只从最简单的一个开始：

	@model CommentViewModel

	@using(Html.BeginUmbracoForm("CreateComment", "BlogPostSurface"))
	{
		@Html.EditorFor(x => Model)
		<input type="submit"/>
	}

上面的代码片段是用于输出表单的局部视图。由于视图的模型就是我们想要承载的ViewModel，我们可以使用`@Html.EditorFor(x => Model)`来自动建立所有的输入字段。

#### BeginUmbracoForm 重载 ####

这是BeginUmbracoForm可用的不同重载：

	//与上面的示例一样
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName)
	
	//接下来的三个类似于上面，但是允许你指定额外的路由值以及form 标签的 html 属性
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName)
	
	// The next three are the same as above but allow you to specify additional route values and/or html attributes for the form tag
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName, object additionalRouteVals)
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName, object additionalRouteVals, object htmlAttributes)
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName, object additionalRouteVals, IDictionary<string, object> htmlAttributes)
	
	//允许你指定 action 名字以及控制器类型：
	BeginUmbracoForm(this HtmlHelper html, string action, Type surfaceType)
	BeginUmbracoForm<T>(this HtmlHelper html, string action)
	BeginUmbracoForm(this HtmlHelper html, string action, Type surfaceType, object additionalRouteVals)
	BeginUmbracoForm<T>(this HtmlHelper html, string action, object additionalRouteVals)
	BeginUmbracoForm(this HtmlHelper html, string action, Type surfaceType, object additionalRouteVals, object htmlAttributes)
	BeginUmbracoForm<T>(this HtmlHelper html, string action, object additionalRouteVals, object htmlAttributes)
	BeginUmbracoForm(this HtmlHelper html, string action, Type surfaceType, object additionalRouteVals, IDictionary<string, object> htmlAttributes)
	BeginUmbracoForm<T>(this HtmlHelper html, string action, object additionalRouteVals, IDictionary<string, object> htmlAttributes)
	
	//接下来的仅适用于基于插件的 surface 控制器。如果你不想指定控制器类型，你可以指定area，插件会寻找匹配的SurfaceController
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName, string area)
	BeginUmbracoForm(this HtmlHelper html, string action, string controllerName, string area, object additionalRouteVals, IDictionary<string, object> htmlAttributes)

## 理解路由处理 ##
很多人都尝试从他们的[HttpPost]action 中直接返回局部视图，是由于没有充分理解路由上传数据到服务器的过程。这里将解释事件的顺序。 这里例子中我们假设这个页面输出的地址是：http://mysite.com/feedback

### 1. Umbraco 页面请求 ###

1. 用户浏览页面 http://mysite.com/feedback
2. Umbraco 找到这个内容页面

### 2. Umbraco 页面输出 ###
1. RenderMvcController 为当前页面执行请求
2. MVC 视图会输出它包含的局部视图，局部视图会通过`BeginUmbracoForm`来创建 html 表单

### 3. 用户提交表单 ###

1. 用户填写表单并提交
2. 生成 http 的 POST 请求 (你要注意到和当前输出一样的 URL 地址: http://mysite.com/feedback)
3. Umbraco 找到内容页面
4. Umbraco 检测到 POST 请求被创建
5. Umbraco 通过BeginUmbracoForm 加密特定的隐藏值注入到 POST 中
6. 使用这个加密值路由到请求指定SurfaceController's 的action 
7. [HttpPost] action如果未验证返回"CurrentUmbracoPage()"，如果验证则返回"RedirectToCurrentUmbracoPage()"...

	#### 3.1 RedirectToCurrentUmbracoPage()
	
	1. 请求完全重定向, **1. Umbraco 页面请求**这个过程又开始了

	#### 3.2 CurrentUmbracoPage()

	1. 如果请求有任何模型数据验证错误，都会自动添加到ModelState 字典。你还可以手工添加任何错误到ModelState 字典。
	2. 调用`return CurrentUmbracoPage()`通过 Umbraco 管道发送请求回执并且保持当前的ModelState和 ViewData 。
	3. **2. Umbraco 页面输出**这个过程又开始了

因此你可以看到如果你从您的[HttpPost]action 内部返回局部视图，唯一发生的事情就是最终只会显示局部视图的标记给你的终端用户，因为你没有发送请求到 Umbraco。

## 显示成功信息

显示一条成功信息是简单的。你可以将用户定向到完全不同的页面。例如，你可以使用`return RedirectToUmbracoPage(nodeId)`。此页通常是窗体所在的页的子项，该页在导航中是隐藏的。

其他的方法是添加信息或其他什么到 TempData。例如你可以在 SurfaceController 的 action 中使用`TempData.Add("Success", "your form was successfully submitted");`，然后通过`@TempData["Success"]`将信息显示在表单定位的页面中。
