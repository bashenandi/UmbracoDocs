<<<<<<< HEAD
# 使用局部视图创建 MVC 表单
=======
# Creating an MVC form using a Partial View
>>>>>>> upstream/master

**适用于: Umbraco 4.10.0+**

_这个教程会解释如何在 Umbraco 中使用局部视图来创建一个表单。这可能是在 Umbraco 中创建 MVC 表单最简单的方法。_

<<<<<<< HEAD
## View Model
=======
## The View Model
>>>>>>> upstream/master

将在本教程中使用的视图模型如下:
	
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


<<<<<<< HEAD
这个类定义的数据是要提交的数据，还定义了哪些数据是必填的，MVC 会自动实现这些验证属性，前端验证 JS 也会自动执行。
=======
## The Surface Controller
>>>>>>> upstream/master

## Surface Controller
为了这个教程，我们创建的 Surface 控制器包含一个 action 用来接收从表单 POST过来的值。在这个示例中，这个 action 会：

<<<<<<< HEAD
*	检查模型是否是有效的 - 基于上面基于验证属性的模型，我们不会执行任何自定义验证
*	如果模型 **是无效的**, 返回到当前绘制的 Umbraco 页面（不做重定向）。通过不跳转，ViewData还是保存在 ModelState 中的，好包含有验证信息。
*	如果模型 **是有效的**,添加一段自定义消息到 TempData 集合，并重定向到当前绘制的 Umbraco 页面。对于网页来说如果POST 是成功的，重定向是标准的处理过程。这会确保 POST 数据不会因意外的按下了 F5（刷新）而意外的重复提交... *不幸的是ASP.Net WebForms并不会遵循规则*
=======
*	Check if the model is valid - based on the validation attributes applied to the model above, we will not be performing any custom validation
*	If the model **is not valid**, return the currently rendered Umbraco page (do not redirect). By not redirecting the ViewData is preserved including the ModelState which contains the validation information.
*	If the model **is valid**, add a custom message to the TempData collection and then redirect to the currently rendered Umbraco page. A standard procedure for a web based for is to redirect if the POST is successful. This ensures that the POST cannot be accidentally re-submitted by accidentally pressing F5 (refresh) ... *unfortunately ASP.NET WebForms does not adhere to this rule by default but it 'should' be done in WebForms too.* 
>>>>>>> upstream/master


<<<<<<< HEAD
		public class BlogPostSurfaceController : Umbraco.Web.Mvc.SurfaceController
		{
			[HttpPost]
			public ActionResult CreateComment(CommentViewModel model)
			{
				//model not valid, do not save, but return current Umbraco page
				if (!ModelState.IsValid)
				{
					//Perhaps you might want to add a custom message to the ViewBag
					//which will be available on the View when it renders (since we're not redirecting)	
					return CurrentUmbracoPage();
				}
				
				//Add a message in TempData which will be available 
				//in the View after the redirect 
				TempData.Add("CustomMessage", "Your form was successfully submitted at " + DateTime.Now)
				
				//redirect to current page to clear the form
				return RedirectToCurrentUmbracoPage();
			}
=======
	public class BlogPostSurfaceController : Umbraco.Web.Mvc.SurfaceController
	{
		[HttpPost]
		public ActionResult CreateComment(CommentViewModel model)
		{    
		    // model not valid, do not save, but return current Umbraco page
		    if (!ModelState.IsValid)
			{
				// Perhaps you might want to add a custom message to the ViewBag
				// which will be available on the View when it renders (since we're not 
				// redirecting)	    	
		   		return CurrentUmbracoPage();
			}
				    
			// Add a message in TempData which will be available 
			// in the View after the redirect 
			TempData.Add("CustomMessage", "Your form was successfully submitted at " + DateTime.Now)
		
		    // redirect to current page to clear the form
		    return RedirectToCurrentUmbracoPage();		    
>>>>>>> upstream/master
		}

<<<<<<< HEAD
## 创建局部视图输出表单

在 MVC 中绘制表单最好的方式，是使用强类型模型的局部视图来绘制。在本例中，我们在*~/Views/Partials/BlogCommentForm.cshtml*创建一个使用了前面创建的强类型模型的局部视图。这个示例展示了如何使用带强类型的BeginUmbracoForm重载方法来指定提交到哪个 Surface 控制器和 Action。
=======
## Create a Partial View to render the form
>>>>>>> upstream/master

为了简化，我们使用`Html.EditorFor(x => Model)`自动填充所有模型的字段，但是如果你需要更加细致的控制标记，你可以分别创建输入字段。

	@model CommentViewModel

	@using(Html.BeginUmbracoForm<BlogPostSurfaceController>("CreateComment"))
	{
		@Html.EditorFor(x => Model)
		<input type="submit"/>
	}

<<<<<<< HEAD
## 输出局部视图
=======
## Render the Partial View
>>>>>>> upstream/master

最后一步是在你的 Umbraco 模板视图中绘制我们刚才创建的局部视图：

	@Html.Partial("BlogCommentForm")

你还可以通过预设模型来给表单预设字段。例如：

	@Html.Partial("BlogCommentForm", new CommentViewModel() { Name = "Some guy" })