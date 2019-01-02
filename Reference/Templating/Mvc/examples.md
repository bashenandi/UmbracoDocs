# 视图/Razor 示例

_在视图中使用各种技术呈现数据的一些例子_ 

## 使用UmbracoHelper输出字段

	@Umbraco.Field("bodyContent")

## 使用UmbracoHelper可选参数输出字段

	@Umbraco.Field("bodyContent", insertBefore : "<h2>", insertAfter : "</h2>")

## 从IPublishedContent 中输出字段原始值

	@Model.Content.Properties["bodyContent"].Value

Or alternatively:

	@Model.Content.GetPropertyValue("bodyContent")

## 从IPublishedContent 中输出字段转换值

 	@Model.Content.GetPropertyValue<double>("amount")
	@Model.Content.GetPropertyValue<RawXElement>("xmlContents")

## 使用@CurrentPage输出字段(动态语句)

	@CurrentPage.bodyContent

## 输出宏

	@Umbraco.RenderMacro("myMacroAlias")

## 输出带匿名对象参数的宏

	@Umbraco.RenderMacro("myMacroAlias", new { name = "Ned", age = 28 })

## 输出带字典对象参数的宏

	@Umbraco.RenderMacro("myMacroAlias", new Dictionary<string, object> {{ "name", "Ned"}, { "age", 27}})

## 输出一些会员数据

	@if(Members.IsLoggedIn()){
	   var profile = Members.GetCurrentMemberProfileModel();
	   var umbracomember = Members.GetByUsername(profile.UserName);
	   
	    <h1>@umbracomember.Name</h1>
	    <p>@umbracomember.GetPropertyValue<string>("bio")</p>
	}


