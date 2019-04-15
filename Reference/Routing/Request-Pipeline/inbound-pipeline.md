# 入站请求管道 #

入站进程由Umbraco（HTTP）模块触发。[已发布内容请求准备](123) 进程启动并创建一个`PublishedContentRequest`。

`PublishedContentRequest`对象代表 Umbraco 必须处理的请求。它包含渲染它所需的所有内容。所有这一切都在 Umbraco 模块认为它的一个文档要渲染时发生。

    public class PublishedContentRequest
    {
      public Uri Uri { get; }
      …
    }

这里有4个重要的属性，其中包含了查找返回节点的所有信息。
  
    public bool HasDomain { get; }
    public Domain Domain { get; }
    public Uri DomainUri { get; }
    public CultureInfo Culture { get; }

域名包含："example.com"，Uri 就是 "http://example.com"。

它还包含要渲染的内容：

    public bool HasPublishedContent { get; }
    public IPublishedContent PublishedContent { get; set; }
    public bool IsInitialPublishedContent { get; }
    public IPublishedContent InitialPublishedContent { get; }
    public void SetIsInitialPublishedContent();
    public void SetInternalRedirectPublishedContent(IPublishedContent content);
    public bool IsInternalRedirectPublishedContent { get; }

包含模板信息以及相应的渲染引擎：

    public bool HasTemplate { get; }
    public string TemplateAlias { get; }
    public RenderingEngine RenderingEngine { get; }
    public bool TrySetTemplate(string alias);
    public void SetTemplate(ITemplate template);

您可以订阅事件来了解`PublishedContentRequest`何时处理完毕。您可以更改任何内容（内容，模板，……）

    // public static event EventHandler<EventArgs> Prepared;
    PublishedContentRequest.Prepared += (sender, args) =>
    {
      var request = sender as PublishedContentRequest;
      // do something…
    }
