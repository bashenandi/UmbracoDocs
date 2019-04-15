# IContentFinder #

如果你想创建你自己的内容查询器实现IContentFinder接口：

	public interface IContentFinder
	{
	  bool TryFindContent(PublishedContentRequest contentRequest);
	}
	
    // Some are registered by default
	// But feel free to use your owns
	public class ContentFinderResolver
	{ … }

Umbraco 会返回所有的内容查询器，在遇到第一个返回 true 的时候停止。查询器可以设置内容，模板，重定向……

### 示例  ###

    public class MyContentFinder : IContentFinder
    {
      public bool TryFindContent(PublishedContentRequest contentRequest)
      {
        var path = contentRequest.Uri.GetAbsolutePathDecoded();
        if (!path.StartsWith("/woot"))
        return false; // not found

        // have we got a node with ID 1234?
        var contentCache = contentRequest.RoutingContext.UmbracoContext.ContentCache;
        var content = contentCache.GetById(1234);
        if (content == null) return false; // not found

        // render that node
        contentRequest.PublishedContent = content;
        return true;
      }
    }

### 默认的内容查询示例 ###

    public class ContentFinderByNiceUrl : IContentFinder
    {
      public virtual bool TryFindContent(PublishedContentRequest contentRequest)
      {
        string path = contentRequest.HasDomain
          // eg. 5678/path/to/node
          ? contentRequest.Domain.RootNodeId.ToString() + …
          // eg. /path/to/node
          : contentRequest.Uri.GetAbsolutePathDecoded();
      
        var node = FindContent(contentRequest, path);
        return node != null;
      }
    }

默认的查询器会查找域名根下面的内容。这是始终如一的。

### 示例 ###

本示例显示了如何添加自定义内容查询器（以及如何从中移除ContentFinderByNiceUrl）到ContentFinderResolver。

    public class MyApplication : ApplicationEventHandler
    {
      protected override void ApplicationStarting(…) 
      {
        // Insert my finder before ContentFinderByNiceUrl
        ContentFinderResolver.Current
          .InsertTypeBefore<ContentFinderByNiceUrl, MyContentFinder>();

        // Remove ContentFinderByNiceUrl
        ContentFinderResolver.Current.RemoveType<ContentFinderByNiceUrl>();
      }
    }

## NotFoundHandlers ##

要设置自己的404查询器，请创建一个IContentFinder并将其设置为ContentLastChanceFinder。

ContentLastChanceFinder会一直返回一个404状态码。此时里创建一个新的IContentFinder实现

此示例创建IContentFinder的新实现，并使用`PublishedContentRequest`类中提供的默认`Is404`属性检查是否无法找到请求的内容。

    public class My404ContentFinder : IContentFinder {
    	public bool TryFindContent(PublishedContentRequest contentRequest) {
            // logic to find your 404 page and set it to contentRequest.PublishedContent
	     CultureInfo culture = null;
            if (contentRequest.HasDomain) {
                culture = CultureInfo.GetCultureInfo(contentRequest.UmbracoDomain.LanguageIsoCode);
            }

            // replace 'home_doctype_alias' with the alias of your homepage
            IPublishedContent rootNode = contentRequest.RoutingContext.UmbracoContext.ContentCache.GetByXPath("root/home_doctype_alias").FirstOrDefault(n => n.GetCulture().ThreeLetterWindowsLanguageName == culture.ThreeLetterWindowsLanguageName);
            // replace '404_doctype_alias' with the alias of your 404 page
            IPublishedContent notFoundNode = contentRequest.RoutingContext.UmbracoContext.ContentCache.GetByXPath(String.Format("root/homeDocType[id={0}]/404_doctype_alias", rootNode.Id)).FirstOrDefault(n => n.GetCulture().ThreeLetterWindowsLanguageName == culture.ThreeLetterWindowsLanguageName);

            if (notFoundNode != null) {
                contentRequest.PublishedContent = notFoundNode;
            } else if (rootNode != null) {
                contentRequest.PublishedContent = rootNode;
            } else {
                contentRequest.PublishedContent = contentRequest.RoutingContext.UmbracoContext.ContentCache.GetAtRoot().FirstOrDefault(n => n.GetTemplateAlias() != "");
            }

            return contentRequest.PublishedContent != null;
	    }
    }
    
关于如何注册自己的实现的示例：

    ContentLastChanceFinderResolver.Current.SetFinder(new My404ContentFinder());
