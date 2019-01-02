# 使用自定义 html 标记创建一个 MVC 表单

**适用于: Umbraco 4.10.0+**

_这个教程会展示如何使用自定义标记创建一个表单（不是自动填充）_

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

对于本教程，我们将要创建的Surface 控制器包含一个 action，用于接受从表单 POST 的值。在本例中，action 会：

*	检查模型是否是有效的 - 基于上面基于验证属性的模型，我们不会执行任何自定义验证
*	如果模型 **是无效的**, 返回到当前绘制的 Umbraco 页面（不做重定向）。通过不跳转，ViewData还是保存在 ModelState 中的，好包含有验证信息。 *(查看下面的'注意事项'获取更多信息)*
*	如果模型 **是有效的**, 添加一段自定义消息到 TempData 集合，并重定向到当前绘制的 Umbraco 页面。对于网页来说如果POST 是成功的，重定向是标准的处理过程。这会确保 POST 数据不会因意外的按下了 F5（刷新）而意外的重复提交... *不幸的是ASP.Net WebForms并不会遵循规则*

<br/>

	public class BlogPostSurfaceController : Umbraco.Web.Mvc.SurfaceController
	{
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

## 创建一个局部视图来绘制表单



在 MVC 中绘制表单最好的方式，是使用强类型模型的局部视图来绘制。在本例中，我们在*~/Views/Partials/BlogCommentForm.cshtml*创建一个使用了前面创建的强类型模型的局部视图。这个示例展示了如何使用带强类型的BeginUmbracoForm重载方法来指定提交到哪个 Surface 控制器和 Action。我们会手工创建标记，而不是 MVC 自动生成表单。

	@model CommentViewModel

	@using(Html.BeginUmbracoForm<BlogPostSurfaceController>("CreateComment"))
	{
		<div class="editor-label">
	        @Html.LabelFor(x => Model.Name)
	    </div>
	    <div class="editor-field">
	        @Html.TextBoxFor(x => Model.Name)
	        @Html.ValidationMessageFor(x => Model.Name)
	    </div>
	 
	    <div class="editor-label">
	        @Html.LabelFor(x => Model.Email)
	    </div>
	    <div class="editor-field">
	        @Html.TextBoxFor(x => Model.Email)
	        @Html.ValidationMessageFor(x => Model.Email)
	    </div>
	    
	    <div class="editor-label">
	        @Html.LabelFor(x => Model.Comment)
	    </div>
	    <div class="editor-field">
	        @Html.TextAreaFor(x => Model.Comment)
	        @Html.ValidationMessageFor(x => Model.Comment)
	    </div>

		<input type="submit"/>
	}

这有许多[HtmlHelper 方法](http://msdn.microsoft.com/en-us/library/system.web.mvc.htmlhelper_methods(v=vs.108).aspx)，你可以用来绘制表单。上面我们使用的是强类型帮助方法：`LabelFor`, `TextBoxFor`, `TextAreaFor` 和 `ValidationMessageFor`。

## 绘制局部视图

最后一步是在 Umbraco 模板视图中绘制刚才创建的局部视图：

	@Html.Partial("BlogCommentForm")

你还可以通过预设模型来给表单预设字段。例如：

	@Html.Partial("BlogCommentForm", new CommentViewModel() { Name = "Some guy" })