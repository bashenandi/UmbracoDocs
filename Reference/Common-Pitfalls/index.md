# 常见陷阱与反模式 #

_这部分格外重要！它描述了开发人员容易陷入的许多的常见陷阱。
这里提到的一些反模式会使你的站点停止，内存泄漏，运行不稳定或者性能下降。
确保你阅读了这个部分 - 它可能会挽救你的网站！_

## 使用单例模式和静态对象 ##
一般来说如果你编写软件已经一段时间，你应该会使用到依赖注入规则。
如果你这么做了，你可能不再使用单例模式和静态对象（多数时候也不必这么做！），由于 Umbraco 自身并不是一个可以开箱使用的IoC容器，你可以会用 Umbraco 构建一些单例单例访问器，例如：`ApplicationContext.Current`和`UmbracoContext.Current`。在多数时候，你不该使用这些单例操作器，它会使你的代码难于测试，而更加重要的是使用单例模式和静态对象会使你的代码难于管理，APIs 会容易泄露最终你会遇到比开始时更多的问题。

在所有你平常使用的基于 Umbraco 的类中，有些对象已经作为属性暴露，所以请使用它们来替换！
例如，所有 Umbraco 创建的Razor 视图，都会将UmbracoContext暴露为一个`UmbracoContext `属性，它们暴露`ApplicationContext `属性指向 Umbraco 的ApplicationContext。在其他基类中也会暴露所有你需要的实例，例如：`SurfaceController`,
`UmbracoApiController`, `UmbracoController`, `RenderMvcController`, `UmbracoUserControl`, `UmbracoPage`, `UmbracoHttpHandler`，这个支持列表还会添加

_用基类属性替代单例访问器的示例：__

_这个示例展示了在`SurfaceController`中不依赖单实例来操作所有Umbraco 服务。这些类似的属性存在于所有你常用的 Umbraco 的基类中，包括 Razor 视图。_

	public class ContactFormSurfaceController: SurfaceController{
		[HttpPost]
		public ActionResult SubmitForm(ContactFormModel model){
			//TODO: 正常的表单逻辑处理代码放在这里
			// 你可以操作所用的，因为它们是基类的属性,
			// 注意这里没有单例模式操作!
			
			using (ProfilingLogger.TraceDuration<ContactFormSurfaceController>("start", "stop")){
				// Logger:
				Logger.Warn<ContactFormSurfaceController>("warning!");
				
				// MembershipHelper:
				Members.CurrentUserName;
				
				// ServiceContext:
				Services.ContentService.GetById(1234);
				
				// ApplicationContext:
				ApplicationContext.ApplicationCache.RuntimeCache.GetCacheItem("myKey", () => "hello world");
				
				// UmbracoContext:
				UmbracoContext.UrlProvider.GetUrl(4321);
				
				// DatabaseContext:
				DatabaseContext.Database.ExecuteScalar<int>("SELECT COUNT(*) FROM umbracoNode");
				
			}
		}
	}
	
所以当你下一次使用`ApplicationContext.Current` 或者 `UmbracoContext.Current`时思考一下"我为什么这么做？"，
"这是否已经作为我使用的基类的属性公开了？"，"我是使用了依赖注入，我应该注入这些示例到我的类中。"


##  web 请求实例静态引用 (例如 `UmbracoHelper`)

__示例 1:__

`private static _umbracoHelper = new UmbracoHelper(UmbracoContext.Current);`

这个实例在使用`_umbracoHelper`实例操作不一致的数据结果，会发生内存持续泄露的问题。

__为什么?__
理解基于请求的生命周期的对象和基于应用程序的生命周期的对象之间的区别是非常重要的……
这里是一些要点：

* Application 范围 - 如果一个对象是应用程序范围/生命周期的，意味着这个单独的对象实例会存在于整个应用程序的生命周期。这个单独的实例会在每个操作它的线程之间共享。静态变量一直是应用程序范围/生命周期的。

* Request 范围 - 网络世界是由请求组成的，每个请求都是它自己的线程。当一个对象是请求范围/生命周期的，网络请求存活它也就能存活，在网络请求结束后，它会通过垃圾回收机制从内存中销毁或清理。请求范围对象实例，不能在应用程序的其它线程内使用 - __除非你做了类似上面的事__

一个应用程序范围实例的示例，是 Umbraco 的`ApplicationContext `，这个单独的实例会存在于应用程序的生命周期内并可以被其他线程使用。

一个请求范围实例的示例，是`HttpContext `，这个对象存在于单个的请求，它的定义不允许它在其他线程尤其是其他请求线程之间共享，因为这是对应用户安全信息存储的！

`UmbracoContext `也是一个请求范围对象 - 实际上它直接依赖于一个`HttpContext `实例。`UmbracoHelper`是一个请求范围对象 - 正如你在上面看到的，它依赖`UmbracoContext `实例。

因此...在上面的示例中，`UmbracoHelper `依赖于`UmbracoContext `，`UmbracoContext `又依赖于`HttpContext `，因此现在静态赋值给了变量，这意味着这些特殊的请求范围对象现在上升为了应用程序范围生命周期，并且永远不会消失。这还意味着在某些情况下，在内存中的Umbraco缓存备份会卡住，或者那些包含某个用户安全信息的上下文`Security `属性会被多个线程的其他请求所操作！

__其他示例:__

    private static _umbracoContext = UmbracoContext.Current;


    //MembershipHelper 也是个请求范围实例 - 它依赖于UmbracoContext 或者HttpContext 
    // MembershipHelper is also a request scoped instance - it relies either on an UmbracoContext or an HttpContext
    private static _membershipHelper = new MembershipHelper(UmbracoContext.Current);

    private static _request = HttpContext.Current.Request;

## 使用 Descendants 和 DescendantsOrSelf查询

当你使用这些请求时并不是100%不好的，你只是需要理解它可能引发的后果。这有一些特别不好的状况：
你有10,000个内容条目在内容树中，你的树结构看起来是这样的：

    - Root
    -- Home
    --- Blog (list view with 9495 nodes)
    --- Office Locations (list view with 500 nodes)
    --- About Us
    --- Contact Us

你在首页创建了一个菜单，类似于：

    <ul>
        <li><a href="@Model.Content.Site().Url">@Model.Content.Site().Name</a></li>
        @foreach(var node in Model.Content.Site().DescendantsOrSelf().Where(x => x.Level == 2)) 
        {
            <li><a href="@node.Url">@node.Name</a></li>
        }
    </ul>

只是渲染出： _Home, Blog, Office Locations, About Us, Contact Us_

但是! ...  这是最糟糕的做法。这个操作会去遍历 Umbraco 中每个单独的节点，10,000中的每一个。更进一步，意味着会在内存中分配10,000个`IPublishedContent `实例，用于检查它们的`Level`值。

可以这样重写，让其变得简单：

    <ul>
        <li><a href="@Model.Content.Site().Url">@Model.Content.Site().Name</a></li>
        @foreach(var node in Model.Content.Site().Children) 
        {
            <li><a href="@node.Url">@node.Name</a></li>
        }
    </ul>

在许多情况下，你可能知道只有少量的查询结果或者你比较喜欢使用Descendants或DescendantsOrSelf，但是在你编写的时候你要意识到这样做的后果是很重要的。

## 太多查询 (重复查询)
查询内容并不是免费的！当您查询或解析一个属性值时，请注意性能开销。
您可以尝试将每一个查询看作是一个SQL调用 - 您不会想要太多，否则您网站的性能就会受到影响。

这里可以看到一些问题。继续之前的菜单示例，在这个示例中菜单继续使用当前页面节点进行绘制：

    <ul>
        <li><a href="@Model.Content.Site().Url">@Model.Content.Site().Name</a></li>
        @foreach(var node in Model.Content.Site().Children) 
        {
            <li><a href="@node.Url">@node.Name</a></li>
        }
    </ul>

语法`@Model.Content.Site()`实际上是`Model.Content.AncestorsOrSelf(1)`的短格式，这意味着它不得不向上查询，一直到父级根节点为止。上面提到过，穿越查询成本开销是比较大的，而上例中为同一个值使用了3次。我们替换为下面的代码：

    @{
        var site = Model.Content.Site();
    }
    <ul>
        <li><a href="@site.Url">@site.Name</a></li>
        @foreach(var node in site.Children) 
        {
            <li><a href="@node.Url">@node.Name</a></li>
        }
    </ul>

## 动态语句
在 Umbraco 8+中，移除了操作IPublishedContent动态支持。这里有一些原因：

* 与强类型相比，动态类型是非常缓慢的
* 代码基础的动态语句对于维护是困难和繁重的
* 动态类型中的许多查询概念是很难理解的，而且需要记住所有需要的字符串语法
* 如果发生错误时，它们的语法是非常难于调试的
* 在Visual Studio中没有智能感知
* ModelsBuilder和 Umbraco 核心很重要的一部分，提供了许多友好和健壮类型的操作用于在视图中的属性操作和查询

你如何知道你是否使用了动态语句？

* 如果你使用了`@CurrentPage` 那么 __你就是__ 使用了动态语句
* 如果你使用了UmbracoHelper查询方法，类似于`@Umbraco.Content`或 `@Umbraco.Media`替代了强类型方法`@Umbraco.TypedContent` 和 `@Umbraco.TypedMedia`，那么 __你就是__ 使用了动态语句

强烈建议你在视图中使用强类型的`@Model.Content`替换`@CurrentPage`模型，事实上这会提高性能并且Umbraco 会将其当做查询`IPublishedContent `来向前兼容。

一个大问题是，当你使用了字符串转换语句类似于：`@CurrentPage.Children.Where("DocumentTypeAlias == \"DatatypesFolder\" && Visible")`当你替换它们时需要编写一些语句来兼容它们。

__关于 Query Builder 需要注意的:__ _我们意识到目前后台的模板编辑器中的 Query Builder 使用了动态语句。我们最终将照着这个最佳实践的建议，使用强类型模型（Models Builder）语句替换对话框中的查询逻辑。同时如果你担心性能并拥有大型站点，那么我们建议您使用查询生成器以强类型语法更新其结果。_

## 在你的视图中使用服务层

Umbraco 中的服务层，是用来用于 Umbraco 的业务逻辑直接存取数据库的。这些方法不该在你的视图中使用，可能对应用程序的稳定性和性能有很大的影响。

你的视图应该仅使用只读的数据操作`UmbracoHelper `和它暴露的属性和方法。这将确保数据的查询变得快速（从缓存中）以免你无意中更改数据库中的数据。

__例如__ 在视图中检索数据:


    //视图中的服务操作 :(
    var dontDoThis = ApplicationContext.Services.ContentService.GetById(123);

    //视图中的内容缓存操作 :)
    var doThis = Umbraco.TypedContent(123);

如果你在视图中使用了`Application.Services...`，你应该弄清楚为什么这么做，而且多数时候你应该删除这些逻辑。

__For example__, when retrieving a content item in your views:

    // Services access in your views :(
    var dontDoThis = ApplicationContext.Services.ContentService.GetById(123);

    // Content cache access in your views :)
    var doThis = Umbraco.TypedContent(123);

If you are using `Application.Services...` in your views, you should figure out why this is being done and, in most cases, remove this logic.   


## 使用UmbracoContext来操作ApplicationContext
你不应该通过`UmbracoContext`来操作`ApplicationContext`。

例如 `UmbracoContext.Current.Application` _<-- 现在这是过时的_

如果你需要操作`UmbracoContext`和`ApplicationContext`，你应该通过下面的一种：

* 通过你使用的 Umbraco 基类所暴露的属性来操作这些服务 (例如. Controllers, views, controls, http handler, etc...)
* 或者注入这些服务类到你使用的服务中 
* 或者从这些服务自己的单例模式中操作这些服务：`UmbracoContext.Current` 和 `ApplicationContext.Current`。

The reason why this is bad practice is that it has caused confusion and problems in the past. In some cases developers would always
access the `ApplicationContext` from the `UmbracoContext` but as we now know, this won't always work because the `UmbracoContext` is a request

scoped instances which isn't going to be available when executing code in a non-request scope (i.e. background thread).（懒得翻译了，一个意思，不该通过web 请求级别的生命周期去获取应用程序级别的）

## 为不稳定的数据使用 Umbraco 内容条目 

是一个最坏的Umbraco反模式，很可能导致你的站点性能超低。

Umbraco 的内容不应该用于不稳定的数据，Umbraco 的 APIs 和 Umbraco 的数据从来都不是为此而设计的。如果你需要存储/写入/跟踪更改频繁的数据，你应该使用自定义数据库表或者其他服务，而不能是 Umbraco 内容节点。

一些不该这么做的示例：

* 点击计数或者页面访问统计 - 使用类似Google Analytics或者自定义数据表
* 为表单提交创建新节点 - 这应该存储在自定义数据库表中
* 导入大量数据到 Umbraco 内容节点中要比存储在自定义数据表中更容易（例如，不会编辑它）。在某些时候这是 ok 的，但是更多时候我们每小时/每周批量导入就最好避免。


## 启动进程
Umbraco 允许你在通过`ApplicationEventHandler `在启动期间运行一些初始化代码，但你无必要确认这样并不会拖慢应用程序的启动速度。

作为包开发人员，您应该特别小心，因为您的包可能是用在数以千计的网站上，你不能减慢应用程序启动的速度。

在许多时候，[初始化代码延迟加载替换提前加载](https://msdn.microsoft.com/en-us/library/dd997286(v=vs.110).aspx)。只有在应用程序启动需要的时候执行你的初始化代码而不是启动时就加载。

这有多种途径可以实现：

* 使用 [`Lazy<T>`](https://msdn.microsoft.com/en-us/library/dd642331(v=vs.110).aspx) 将初始化逻辑放在它的回调中
* 使用 [`LazyInitializer`](https://msdn.microsoft.com/en-us/library/system.threading.lazyinitializer%28v=vs.110%29.aspx?f=255&MSPPError=-2147217396)

更加要的是你要确保初始化逻辑在整个应用程序生命周期内，仅在应用重启时执行一次。如果你的初始化逻辑会创建数据库表或者类似的一些事情，那么它就应该只执行一次，接下来你应该设置一个持续性标识（或者文件）来表明你的初始化代码逻辑已经执行过，不要再次执行。

## 重建索引
太多时候我们看到在用户们的解决方案代码中会重建Exmaine 索引（我们甚至在每个请求上都看到了这一点！）。重建索引会导致严重的性能问题，这不是推荐的方式。Umbraco 和 Examine的索引管理，索引的稳定性和索引中的数据同步在每个版本都运行的很好。如果你的索引数据与 Umbraco 数据不同步，你应该确保你一直运行的是最新版本的 Umbraco 和 Examine。

你的数据不同步，主要是因为：

* 使用了旧版本的
* 同时重启你的 app 应用程序和重建索引(尽量避免这种方案!)

我们不建议你重建索引，除非你有绝对的理由要去这么做，如果您经常需要这样做，那么建议您慎重考虑这样做的原因，并设法解决潜在的问题。

## Examine 事件进行的查询和逻辑

这里有几个常见的 Examine 事件：`GatheringNodeData `和`DocumentWriting `。这些方法都允许开发人员修改 Lucene 索引中的数据，但是很多时候，我们看到开发人员在这些方法中执行服务查询。例如：在这些方法中使用`ApplicationContext.Current.Services.ContentService.GetById(e.NodeId)`会遇到`N + 1`问题。

这是因为这些方法会在每个独立的文档建立索引和你重建索引时执行，这将意味着这些逻辑将介入每个独立文档和媒体条目的每个索引...那将意味着数量巨大的轮询和性能损耗。

## 绘制模板 ##

在 Umbraco 中有一个你永远不该使用的 API，除非你真的真的知道你要做什么。这个 API 方法叫`RenderTemplate `。它允许你能够绘制特定的内容条目的模板并且在响应中获取`string`。在有些时候这是有用的，或许你想在内容条目和它的模板基础上发送一封邮件，但是你必须非常小心，不要为了使用它而使用它。

一般来说这个方法不该用于常规内容的输出。如果滥用，会导致严重的性能问题。对于正常的内容，输出其他内容条目的类型数据，你应该使用局部视图替换。

## 不要把逻辑放在构造函数中
构造函数一般不应执行任何逻辑，它们应该用于设置一些参数值，执行一些非空检查和可能的验证数据，但是在大多数情况下它们不应该执行任何逻辑。

这里有一些为什么会导致巨大的性能问题的原因：

* API 的构造器，并不期望通过其创建的对象，还要去担心性能方面的问题
* 创建一个对象时会在不经意间发生太多次，尤其是在使用 Linq 时


这里示范了如何非常快速的生成这个错误的示例：
你的树结构大致看起来像这样：

    - Root
    -- Home
    --- Recipes (node id = 3251, list view with 5000 nodes)
    --- About Us
    --- Contact Us

你有个自定义模型，看起来是这样的：

    public class RecipeModel : PublishedContentWrapped
    {
        public RecipeModel(IPublishedContent content) : base(content)
        {
            RelatedRecipes = content 
                .Parent           
                .Children
                .Where(x => x.GetPropertyValue<IEnumerable<int>>("related")
                                .Contains(content.Id));

            Votes = content.GetPropertyValue<int>("votes");
        }

        public int Votes { get; private set; }
        public IEnumerable<RecipeModel> RelatedRecipes { get; private set; }
    }
然后你运行下面的代码，显示收藏夹 

    @var recipeNode = Umbraco.TypedContent(3251);
    <ul>
    @foreach(var recipe in recipeNode.Children
                                .Select(x => new RecipeModel(x))
                                .OrderByDescending(x => x.Votes)
                                .Take(10))
    {
        <li><a href="@recipe.Url">@recipe.Name</a></li>    
    }
    </ul>

__Ouch!__ 因此只是显示投票最多的 top10，这将完成以下操作：

* 这将遍历所有的Recipess ，创建并分配5000个`IPublishedContent `实例
* 这将创建并分配5000个`RecipeModel`实例
* 对于每个`RecipeModel`的创建，这会向上横切，递归所有的5000个recipes，然后为两个属性解析属性数据

这意味着目前在内存中至少创建并分配了 __20,000__ 个新对象。遍历和访问它们的次数现在达到了5000 x 5000 = __25,000,000 (2500万次!)__

这会引导我们进入下一个反模式...

## 不要过早载入数据，在需要的时候加载
上面的示例重写为下面的样子：

    public class RecipeModel : PublishedContentWrapped
    {
        public RecipeModel(IPublishedContent content) : base(content)
        {
        }

        private int? _votes;
        public int Votes 
        {
            get 
            {
                // Lazy load the property value and ensure it's not re-resolved once it's loaded
                return _votes ?? (_votes = GetPropertyValue<int>("votes"));
            } 
        }

        // Just return the Ids, they can be resolved to IPublishedContent instances in the view or elsewhere,
        // doesn't need to be in the model - this would also be bad if the model was cached since all of the
        // related entities would end up in the cache too.
        private List<int> _related;
        public IEnumerable<int> RelatedRecipes
        {
            get 
            {
                // Lazy load the property value and ensure it's not re-resolved once it's loaded            
                return _related ?? 
                    (_related = GetPropertyValue<IEnumerable<int>>("related").ToList());
            } 
        }    
    }

这稍微好了一些:

* This will iterate over all Recipes, create and allocate 5000 instances of `IPublishedContent`
* This will create and allocate 5000 instances of `RecipeModel`

This means that there is now a minimum of __15,000__ new objects created and allocated in memory. The number of traversals/visits to each
of these objects is now __5000__.

This is still not great though. There really isn't much reason to create a `RecipeModel` just to use it as a filter,
this is allocating a lot of objects to memory for no real reason. This could just as easily be written like:

    @var recipeNode = Umbraco.TypedContent(3251);
    <ul>
    @foreach(var recipe in recipeNode.Children
                                .OrderByDescending(x => x.GetPropertyValue<int>("votes"))
                                .Take(10))
    {
        <li><a href="@recipe.Url">@recipe.Name</a></li>    
    }
    </ul>

This is slightly better:

This means that there is now a minimum of __10,000__ new objects created and allocated in memory. The number of traversals/visits to each
of these objects is now __5000__.

## 大量的Linq - XPath 依旧对你有帮助 

Based on the above 2 points, you can see that just iterating content with the traversal APIs will cause new
instances of `IPublishedContent` to be created. When memory is used, Garbage Collection needs to occur and this 
turnover can cause performance problems. The more objects created, the more items allocated in memory, the harder the job
is for the Garbage Collector == more performance problems. Even worse is when you allocate tons of items in memory and/or really 
large items in memory, they will remain in memory for a long time because they'll end up in something called "Generation 3" which the 
GC tries to ignore for as long as possible because it knows it's going to take a lot of resources to cleanup!

So, if you have a huge site and are running LINQ queries over tons of content, how do you avoid allocating all of these `IPublishedContent` instances? 

Instead of iterating over (and thus creating them) we can use regular old `XPath` or use the `XPathNodeIterator` directly:

* `UmbracoHelper.ContentQuery.TypedContentAtXPath`
* `UmbracoHelper.ContentQuery.TypedContentSingleAtXPath`
* `UmbracoContext.ContentCache.GetXPathNavigator`

The methods `TypedContentAtXPath` and `TypedContentSingleAtXPath` will return the resulting `IPublishedContent` instances based
on your XPath query but without creating interim `IPublishedContent` instances to perform the query against. 

These 2 methods can certainly help avoid using LINQ (and as such allocating IPublishedContent instances) 
to perform almost any content filtering you want. 

## XPathNodeIterator - 在你需要直接操作XML 时提供支持

Using the `GetXPathNavigator` method is a little more advanced but can come in very handy to solve some performance problems when
dealing with a ton of content. Of course, when you use this method you'll now be working directly with XML.

For example, here's how to turn the above recipe query into a much more efficient query 
without allocating any `IPublishedContent` instances:

    @{
        var recipeNode = Umbraco.TypedContent(3251);
        if (recipeNode == null) throw new NullReferenceException("No node found with ID " + 3251);
        var xPath = $"//* [@isDoc and @id='{recipeNode.Id}']/* [@isDoc]";
    }
    <ul>
    @foreach(var recipe in UmbracoContext.ContentCache.GetXPathNavigator()
                                .Select(xPath).Cast<XPathNavigator>()
                                .OrderByDescending(x =>
                                    {
                                        var vote = 0;
                                        int.TryParse(x.GetAttribute("@id", ""), out vote);
                                        return vote;
                                    })
                                .Take(10))
    {
        <li><a href="@recipe.Url">@recipe.GetAttribute("@nodeName", "")</a></li>    
    }
    </ul>
