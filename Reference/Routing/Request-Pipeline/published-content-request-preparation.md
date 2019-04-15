# Published Content Request Preparation #

在 `UmbracoModule`中称为:

    void ProcessRequest(…)

它做哪些事情：

- 它确保 Umbraco 准备好了，并且请求是一个文档请求
- 创建一个PublishedContentRequest 实例
- 为实例运行PublishedContentRequestEngine.PrepareRequest()
- 处理跳转和状态
- 转发丢失的内容到404
- 转发到 Webforms 或者 MVC

## 准备请求 ##
ProcessRequest方法称为PublishedContentRequestEngine.PrepareRequest 方法。准备请求处理：

- FindDomain()
- Handles redirects
- Sets culture
- FindPublishedContentAndTemplate()
- Sets culture (again, in case it was changed)
- Triggers PublishedContentRequest.Prepared event
- Sets culture (again, in case it was changed)
- Handles redirects and missing content
- Initializes a few internal stuff

下面我们将讨论其中的几个步骤。

### FindDomain() ###
FindDomain查找与请求 Uri 匹配的域

- 使用贪婪(greedy)匹配: “domain.com/foo” 接管 “domain.com”
- 设置已发布内容请求的域
- 如果域名找到了
	- 相应地设置已发布内容请求的区域性
	- 根据当前请求计算域URI ("domain.com" for "http://domain.com" or "https://domain.com")
- 没有找到
- 默认设置已发布内容请求的区域性
(第一个语言，或者系统语言)

### FindPublishedContentAndTemplate() ###
1. FindPublishedContent ()
2. Handles redirects
3. HandlePublishedContent()
4. FindTemplate()
5. FollowExternalRedirect()
6. HandleWildcardDomains()

如果内容没有找到，ContentFinder会介入。在过去这会通过INotFoundHandler来处理，但是新的请求管道使用[IContentFinder](IContentFinder.md)。

更多信息可以在[这里](FindPublishedContentAndTemplate.md)找到。

UmbracoModule会选取重定向设置来重定向……这里不需要编写你自己的重定向：

    PublishedContentRequest.Prepared += (sender, args) =>
    {
      var request = sender as PublishedContentRequest;  
      if (!request.HasPublishedContent) return;

      var content = request.PublishedContent;
      var redirect = content.GetPropertyValue<string>("myRedirect");
      
      if (!string.IsNullOrWhiteSpace(redirect))
        request.SetRedirect(redirect);
    }

## 转发到 Webforms 或者 MVC ##

关于 Webforms - 和 v4版本是一致的（没有变化）。这意味着MVC已经通过管道实现了。

你当然可以创建你自己的 MVC渲染控制器： 


    // 这是默认的控制器
    public class RenderMvcController : UmbracoController
    { … }

    // 但请随意使用自己的
    public class DefaultRenderMvcControllerResolver
    { … }

注意:一个缺少的模板会转到 MVC

默认情况下有一个，但您可以使用自己的，所以花些时间来更改视图…

作为提醒，[Route hijacking](../../../Reference/routing/custom-controllers) 的工作原理是这样的: 

- 创建一个**MyContentType**Controller
  - 将代替默认控制器运行
  - 针对每个**MyContentType**类型的内容
- 如果名称与模板别名匹配，则运行特定操作
- 否则默认（Index）操作将运行

## 丢失模板? ##
如果PrepareRequest找不到模板：

* 它将验证这是否是路由劫持
* 否则，它的处理步骤:
  * HandlePublishedContent()
  * FindTemplate()
  * Handle redirects, etc.
  * Ugly 404 (w/ message)
  * Transfer to WebForms or MVC…

## 其他在流程中被路由的东西 ##
* The /Base Rest service
* WebApi
* Mvc Routes
