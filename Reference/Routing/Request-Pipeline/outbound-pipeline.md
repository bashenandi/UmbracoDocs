# 出站请求管道 #

**出站管道**由以下步骤组成：

1. [Create segments](#segments)
2. [Create paths](#paths)
3. [Create urls](#urls)

为了解释问题，我们将使用以下内容树:

![simple content tree](images/simple-content-tree.png)

## 1. <a name="segments"></a> Create segments
当建立 URL 时，Umbraco 会转换每个节点到一个段（segment）中。每个发布的[Content](../../../Reference/Management/Models/Content)都有一个 url 段。

在我们的例子中"Our Products"会变为"our-products"，"Swibble" 会变为 "swibble"。

段是由"Url Segment provider"创建的

### Url Segment provider ###
在 Umbraco 启动时，`UrlSegmentProviderResolver`会搜索第一个`IUrlSegmentProvider`，它并不会返回 `null`。

如果没有找到，它会返回*default Url segment provider*。

要创建一个新的 Url segment provider, 实现下面的接口：

    public interface IUrlSegmentProvider
    {
      string GetUrlSegment(IContentBase content);
    }

返回的字符串就是你为这个节点生成的 URL 段。你可以自由返回任何你喜欢的字符串，但是它不能包含 url 段分隔符`/`，因为这将会创建额外的『段』。因此例如`5678/swibble`是不被允许的。

#### 示例 ####

    public class MyProvider : IUrlSegmentProvider
    {
      readonly IUrlSegmentProvider _provider = new DefaultUrlSegmentProvider();

      public string GetUrlSegment(IContentBase content)
      {
        if (content.ContentTypeId != 1234) return null;
        var segment = _provider.GetUrlSegment(content);
        return string.Format("{0}-{1}", content.Id, segment);
      }
    }

返回的字符串会转换为原生的 Url 段。你不需要任何 Url 重写，……

如果我们使用`MyProvider`，我们的示例内容树中的"swibble"节点，会拥有"5678-swibble"段。

### The Default Url Segment Provider ###

默认的 Url 构建段看起来是这样的。
首先它会查找（按此顺序）：

- *umbracoUrlName*属性。在节点上使用 `content.GetPropertyValue<string>("umbracoUrlName")`
- content.Name

然后使用 Umbraco 字符串扩展`ToUrlSegment()`来生成一个干净的段。 

    // That one is initialized by default
    public class DefaultUrlSegmentProvider : IUrlSegmentProvider
    { … }

    // Initialized by
    public class UrlSegmentProviderResolver
    { … }

## 2. <a name="paths"></a>Create paths
管道将使用段来创建一个路径。

在我们的示例中，"swibble"节点会接收路径："/our-products/swibble"。如果我们使用了上面的`MyProvider`，路径会变为："/our-products/5678-swibble"。

但是，如果我们添加了其他站点，第二个站点中的节点路径（内部）都将会以该站点中的节点 ID 作为前缀。

任何具有主机名的内容节点都为路径定义一个“新根”。 

![path example](images/path-example.png)

路径会被缓存，而下一个并不会（http vs https, current request…）。

如果你**运行于hostnames**，有一些注意事项：

- **没有路径的域** e.g. "www.site.com"会变为"1234/path/to/page"
- **包含路径的域** e.g. "www.site.com/dk"会生成"1234/dk/path/to/page"路径
- **No domain specified**: "/path/to/page"
- **Unless HideTopLevelNodeFromPath config is true**, then the path becomes "/to/page"

## 3. <a name="urls"></a> Create Urls
节点的URL 是一个完整的[URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)：架构，域名，（端口）和路径。

在我们的实例中，"swibble"节点拥有下面的 URL："http://example.com/our-products/swibble.aspx"。

这个 url 是由Url Provider生成的。每次写入时Url provider 都可以（e.g.）：

	@Content.Model.Url
	@Umbraco.Url(1234)
	@UmbracoContext.Current.UrlProvider.Geturl(1234)

`UrlProviderResolver`会搜索所有 Url 提供器，并且会返回第一个，而不是返回 null。如果没有找到则返回默认的 Urls 提供器。

    // That one is initialized by default
    public class DefaultUrlProvider : IUrlProvider
    { … }
    
    // But feel free to use your own
    UrlProviderResolver.Current.InsertType<MyUrlProvider>();

实现`IUrlProvider`接口可以创建你自己的 Url 提供器

    public interface IUrlProvider
    {
        string GetUrl(UmbracoContext umbracoContext,
            int id,
            Uri current,
            UrlProviderMode mode);
    }

通过GetUrl返回的字符串，可以返回您喜欢的任何内容。

实现自己的提供器是很困难的，建议使用重写默认提供器的方式。如果实现自定义URL提供程序，请考虑以下事项：

- 缓存事项
- 一定要知道如何处理架构（HTTP vs HTTPS）和主机名
- 入站可能要重写

当从DefaultUrlProvider继承时，需要实现指定`IRequestHandlerSection`的构造函数。检索此对象的最简单方法是添加一个构造函数：

    public class MyUrlProvider : DefaultUrlProvider {
      public MyUrlProvider()
        : base(UmbracoConfig.For.UmbracoSettings().RequestHandler)
      { }
      
      public override string GetUrl(UmbracoContext umbracoContext, int id, Uri current, UrlProviderMode mode)
      {
      	 // your own implementation
      }
    }


**TODO: "Per-context UrlProvider".
Stéphane mentions a "per context Url provider" on page 35 of his document.  We need to find out what this is!**

### Url 提供器是如何工作的 ###

- 如果当前域与目标内容的根域相匹配
  - 返回相关的URL
  - 否则必须返回绝对 URL
- 如果目标内容只包含一个根域
  - 使用该域名构成绝对 Url
- 如果目标内容有多个根域
  - 找出使用哪一个
  - 创建绝对URL
- 使用架构完成绝对 Url（http 或 https）
  - 如果域名包含架构就使用它
  - 否则使用当前请求的架构

如果"useDirectoryUrls"设置为 false，则在 URL 中添加.aspx。如果"addTrailingSlash"设置为 true，则添加斜线。然后添加虚拟目录。

如果 URL 提供器在生成内容 URL 时发生冲突，它将会一直选择第一个可用的节点，并为其分配URL。其余的节点将会标记冲突并且不会有 URL生成。如果你尝试获取具有冲突URL的节点的URL，则会得到一个包含节点ID（err-1094）的错误字符串，因为此节点当前没有活动的URL。

如果使用umbracoUrlName属性覆盖节点的生成URL，或者在某些情况下，如果有多个根节点未分配主机名，则可能发生这种情况。

请记住，这意味着使用冲突的URL发布未发布的节点，在已发布的节点现在应根据树中的排序顺序优先处理的情况下，可能会更改正在该特定URL上呈现的活动节点！

### 还有一些事情 ###
**TODO: CHECK WITH IF THIS IS INTERPRETED CORRECTLY.  Copied from page 42 of Stéphane's document.**
 
- IUrlProvider 还有一个GetOtherUrls方法 (用于后端)
- 如果IUrlProvider是`AliasUrlProvider`，还有一个可用接口：在后端显示为umbracoUrlAlias 

### Url 提供程序模式 ###
提供者『模式』决定了绝对还是相对 Url。你可以改变当前提供程序的模式

这是一些不同的模式:

    public enum UrlProviderMode
    {
      // Produce relative Urls exclusively 
      Relative,
      // Produce absolute Urls exclusively
      Absolute,
      // Produce relative Urls when possible, else absolute when required
      Auto,
      // Produce relative Urls when possible, else absolute when required
      // If useDomainPrefixes is true, then produce absolute Urls exclusively
      AutoLegacy // this is the default mode in v6
    }

当useDomainPrefixes设置为 false 时，`Auto`等同于`AutoLegacy`。

*注意*: `UseDomainPrefixes` 在每个模式中都是被忽略的，除了AutoLegacy

默认模式可以在`/umbraco/web.routing/urlProviderMode`中配置

### 网站域名辅助 ###

Url 提供程序需要一个`ISiteDomainHelper`对象，这个对象是由`SiteDomainHelperResolver`提供的。

这个对象返回当前 Uri 以及所有合规的域名，并且仅返回UrlProvider创建 Url 时所使用的那个域名。

    // That one is initialized by default
    public class SiteDomainHelper : ISiteDomainHelper
    { … }
    // But feel free to use your own
    public class SiteDomainHelperResolver
    { … }

创建你自己的网站域名辅助，实现ISiteDomainHelper接口并添加其到解析器（resolver）。

    public interface ISiteDomainHelper
    {
      DomainAndUri MapDomain(Uri current, DomainAndUri[] domainAndUris);
    }

你可以使用默认的SiteDomainhelper添加额外的域名：

    public class MyApplication : ApplicationEventHandler
    {
      protected override void ApplicationStarting(…)
      {
        SiteDomainHelper.AddSite("www", "www.alpha.com", "www.bravo.com");
        SiteDomainHelper.AddSite("staging", "staging.alpha.com", "staging.bravo.com");
      }
    } 

然后它知道当前为“www.alpha.com”时，应该选择的是“www.bravo.com”。

一个更复杂的SiteDomainHelper示例：

    public class MyApplication : ApplicationEventHandler
    {
      protected override void ApplicationStarting(…)
      {
        SiteDomainHelper.AddSite("www", "www.alpha.com", "www.bravo.com");
        SiteDomainHelper.AddSite("mobile", "mobile.alpha.com", " mobile.bravo.com");
        SiteDomainHelper.AddSite("staging", "staging.alpha.com", "staging.bravo.com");
        SiteDomainHelper.BindSites("www", "mobile");
      }
    }

当后端在www.alpha.com/umbraco时链接是"www.bravo.com/bravo-2"；备用链接是"mobile.bravo.com/bravo-2"。

如果你有创建更好的接口的好主意，请在[Umbraco dev group](https://groups.google.com/forum/#!forum/umbraco-dev)中分享它们。