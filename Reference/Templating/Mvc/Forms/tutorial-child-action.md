#使用子 Action 创建 MVC 表单

**适用于: Umbraco 4.10.0+**

_这个教程解释了如何在 Umbraco 中使用子 Action创建表单。_

## View Model
这个教程中使用的视图模型如下：
	
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

## Surface Controller
对于这个教程，我们创建的Surface 控制器包含2个 action，一个用于接收从表单 POST 过来的值，另外一个用于绘制包含表单的视图。 

绘制子 Action会进行的操作：

* 接收一个整数 Id 参数（表示一个会员）
* 查找会员
* 创建CommentViewModel 并且使用会员信息来填充它的 Name 和 Email属性
* 为表单绘制视图

HttpPost Action 用来：

*	检查模型是否是有效的 - 基于上面基于验证属性的模型，我们不会执行任何自定义验证
*	如果模型 **是无效的**, 返回到当前绘制的 Umbraco 页面（不做重定向）。通过不跳转，ViewData还是保存在 ModelState 中的，好包含有验证信息。 *(查看下面的'注意事项'获取更多信息)*
*	如果模型 **是有效的**, 添加一段自定义消息到 TempData 集合，并重定向到当前绘制的 Umbraco 页面。对于网页来说如果POST 是成功的，重定向是标准的处理过程。这会确保 POST 数据不会因意外的按下了 F5（刷新）而意外的重复提交... *不幸的是ASP.Net WebForms并不会遵循规则*

<br/>

	public class BlogPostSurfaceController : Umbraco.Web.Mvc.SurfaceController
	{
		//Important to attribute your child action with ChildActionOnly
		//otherwise the action will become publicly routable (i.e. have
		//a publicly available Url) 
		[ChildActionOnly]
		public ActionResult ShowCommentForm(int memberId) 
		{
			var member = Membership.GetUser(memberId);
			var model = new CommentViewModel();
			if (member != null) 
			{
				model.Name = member.UserName;
				model.Email = member.Email;
			}
			return PartialView("BlogCommentForm", model);
		}

		[HttpPost]
		public ActionResult CreateComment(CommentViewModel model)
		{    
		    //model not valid, do not save, but return current Umbraco page
		    if (!ModelState.IsValid)
			{
				//Perhaps you might want to add a custom message to the ViewBag
				//which will be available on the View when it renders (since we're not 
				//redirecting)	    	
		   		return CurrentUmbracoPage();
			}
				    
			//Add a message in TempData which will be available 
			//in the View after the redirect 
			TempData.Add("CustomMessage", "Your form was successfully submitted at " + DateTime.Now)
		
		    //redirect to current page to clear the form
		    return RedirectToCurrentUmbracoPage();		    
		}
	}

##创建局部视图输出表单

在 MVC 中绘制表单最好的方式，是使用强类型模型的局部视图来绘制。在本例中，我们在*~/Views/Partials/BlogCommentForm.cshtml*创建一个使用了前面创建的强类型模型的局部视图。这个示例展示了如何使用带强类型的BeginUmbracoForm重载方法来指定提交到哪个 Surface 控制器和 Action。

为了简化，我们使用`Html.EditorFor(x => Model)`自动填充所有模型的字段，但是如果你需要更加细致的控制标记，你可以分别创建输入字段。

	@model CommentViewModel

	@using(Html.BeginUmbracoForm<BlogPostSurfaceController>("CreateComment"))
	{
		@Html.EditorFor(x => Model)
		<input type="submit"/>
	}

## 绘制子 Action

最后一步是在不的 Umbraco 模板视图中绘制刚才创建的子 Action：

	@Html.Action("ShowCommentForm", "BlogPostSurface", new { memberId = 1234 })

我们通过 action 名和控制器名。注意当我们调用这个方法时，移除了后缀"Controller"来替代类名BlogPostSurfaceController，我们引用了不带"Controller"后缀的控制器名：*BlogPostSurface*。我们输入一个会员 id （假设为1234）作为参数，通过发送控制器自定义路由值来自动绑定为 actions 的参数。

如果你想，你还可以传入一个对象参数，对于本例我们可以更改代码为传入当前登录的会员对象：

	@Html.Action("ShowCommentForm", "BlogPostSurface", new { member = Membership.GetUser() })

然后更改你的 action为这样：

	[ChildActionOnly]
	public ActionResult ShowCommentForm(MembershipUser member) 
	{
		var model = new CommentViewModel();
		if (member != null) 
		{
			model.Name = member.UserName;
			model.Email = member.Email;
		}
		return PartialView("BlogCommentForm", model);
	}

##Action 命名

当命名 actions 时，你可能会想将它们命名为同一个名字用来绘制表单以及处理表单的 POST 数据。例如上面的例子你可能想这么做：

	[ChildActionOnly]
	public ActionResult CommentForm(int memberId) 
	{
		...
	}

	[HttpPost]
	public ActionResult CommentForm(CommentViewModel model)
	{    
	    ...
	}

然后这可能会导致一些不必要的副作用。如果你在 HttpPost的时候没有做重定向，而是`return CurrentUmbracoPage();`，MVC 会在绘制子action 时中断尝试绘制[HttpPost]方法，这样会导致一些问题。详细信息在这个问题中：[http://issues.umbraco.org/issue/U4-1819](http://issues.umbraco.org/issue/U4-1819)。这个问题是由于MVC路由到了错误的action，因为在这个请求中还有一个HttpPost动词。

如果你真的想要给你的 actons 使用同样的命名，你可以使用一个新的属性，在 Umbraco4.11.6+以及6.0.3是可用的，叫做[NotChildAction]。你可以在你的 HttpPost 方法上添加这个属性，从而避免问题。所以最后你的 actions 看起来是这样的：

	[ChildActionOnly]
	public ActionResult CommentForm(int memberId) 
	{
		...
	}

	[NotChildAction]
	[HttpPost]
	public ActionResult CommentForm(CommentViewModel model)
	{    
	    ...
	}

否则，给你的 actions 使用不同的命名。

## 操作 ViewData 

当你在[HttpPost]方法中添加任何数据到 ViewData 集合时，这些 ViewDta 是在'根'视图上下文上存取的。因为为了从你的 ChildAction 视图中检索 ViewData 中的数据，你需要通过下面的语句来操作：

	@ParentActionViewContext.ViewData

这事实上是由于你渲染 ChildAction 引起的。这与创建普通的 MVC 应用程序时发生的情况完全相同。无论何时你 POST 数据到控制器，你在控制器中做的任何事情，都是在"根"上下文中完成的。

在有些情况下，你可能会从宏中渲染 ChildAction。这种时候可以通过下面的的方式来操作视图数据：

	@ParentActionViewContext.ParentActionViewContext.ViewData
