# 特殊版本升级

*本文档涵盖了一些特定版本升级所需要的升级步骤，大多数版本不需要这些特定的升级步骤，多数时候你可以直接从当前版本升级到最新版本*

先按照常规升级指南中的步骤操作，然后再按照这些特殊版本的附加说明进行操作。 (请记住，如果没有提到新的配置文件，则表示他们已经在一般升级指南中描述过。)


## Version 7.6.3
简述:

在 Umbraco 版本7.6.2，我们在 **Property Value Converts(PVCs)** 中犯了一个错误，在两天后的版本7.6.3中得到更正。如果你在前端查询中查询下列数据类型时遇到问题，请升级至 7.6.3：

 * Multi Node Tree Picker
 * Related Links
 * Member Picker

若果之前尝试修复了这些数据类型的问题，在升级到7.6.3之后您可能需要再次修复它们。

## 属性值转换器?

Umbraco 存储datatypes的数据有多种方式，对于多数的选择类型，可能存储的是（例如）1072或者1083,1283。这些数字指向 Umbraco 中唯一的条目。在过去，当构建模板时，你不得不获取先这个值，然后再查找对应的条目，然后再从其中取出你需要的数据，例如：

	@{
		IPublishedContent contactPage;
		var contactPageId = Model.Content.GetPropertyValue<int>("contactPagePicker");
		if(contactPageId > 0) {
			contactPage = Umbraco.TypedContent(contactPageId);
		}
	}
	<p>
		<a href="@contactPage.Url">@contactPage.Name</a>
	</p>
		
如果"你可以"这样做，是不是会更加友好:

	<p>
		<a href="@Model.Content.ContactPagePicker.Url">@Model.ContactPagePicker.Name</a>
	</p>
		
从 7.6.0 版本开始，由Jeavon 开发的软件包，使用了 Models Builder 并且通过包含的 [core property value converters](https://our.umbraco.org/projects/developer-tools/umbraco-core-property-value-converters/)，使这变成了可能。

为了不破坏大家已有的网站（当启用了 PVCs 会导致查询结果不同），我们默认禁用了 PVCs。

Umbraco 7.6.0 同时使用了新的数据类型[UDI (Umbraco Identifier)](https://our.umbraco.org/Documentation/Reference/Querying/Udi)，来存储拾取器。我们想让这些新的拾取器更加容易使用，所以我们希望默认情况下PVCs 一直为这些拾取器开启。

不幸的是我们注意到有些新的拾取器，当配置项目设置为 **false** 时会禁用 PVCs (`<EnablePropertyValueConverters>false</EnablePropertyValueConverters>`) - 但是内容拾取又会忽略这个设置。

为了使这一切都保持一致，我们确认 UDI 拾取器会在7.6.2版本中一直使用 PVCs……或者只是我们这样认为！我们意外的颠倒了这种行为。所以当 PVCs 启用时，属性不会被转换，而当 PVCs 禁用时，属性随后会被转换。这是与期望完全相反的。Oops！

所以我们在7.6.3中修复了这个问题。

这个问题只影响:

 * Multi Node Tree Picker
 * Related Links
 * Member Picker

如果你已经更新到了7.6.2，并且对于那些数据类型修复了你的查询，你不得不再升级到7.6.3后再次修复这些问题。我们保证这会是你最后一次更新他们！很抱歉给您带来不变。

##Version 7.6.0

7.6.0有了一些破坏性变化，请务必 **[在这里阅读了解](760-breaking-changes.md)** and [跟踪器中这些项目的列表](http://issues.umbraco.org/issues/U4?q=Due+in+version%3A+7.6.0+Backwards+compatible%3F%3A+No+)

三件需要注意的重要问题：

 1. 在 web.config 文件中，如果 `useLegacyEncoding` 当前设置为 `true` ，则 **不要** 设置为 `false` - 改变密码加密规则会让你再也无法登录。
 2. 在 umbracoSettings.config 文件中，保持 `EnablePropertyValueConverters` 设置为 `false` - 这会帮助你已有的内容选取器继续工作
 3. 在 tinyMceConfig.config 文件中，确保删除 `<plugin loadOnFrontend="true">umbracolink</plugin>` ，这样你的富文本编辑器就可以正常工作了

#### 通过 Nuget 更新

这是一个很重要的问题，不幸的是这不是一个完美的解决方案。我们已经删除了不再使用的UrlRewriting依赖，但是如果你想要使用，我们也不想 Nuget 删除你所有的重新规则。好消息是，如果你使用它，NuGet升级并不会删除你的文件，一切都将继续工作（虽然你真的应该使用IIS重写！）。

相反，如果你不再使用它， *你会在升级后获取到一个YSOD，这里告诉你如何修复它**

你从开始如果没有使用过 UrlRewriting，你大改也不会编辑过UrlRewriting文件，因此当 Nuget 检测到这些后会删除它。但是你需要手工从你的 web.config 文件中，删除 UrlRewriting 的相关引用：

* `<section name="urlrewritingnet" restartOnExternalChanges="true" requirePermission="false" type="UrlRewritingNet.Configuration.UrlRewriteSection, UrlRewritingNet.UrlRewriter" />`
* `<urlrewritingnet configSource="config\UrlRewriting.config" />`
* 以及下面的http modules
    	
    	<system.web>
    		<httpModules>
    			<add name="UrlRewriteModule" type="UrlRewritingNet.Web.UrlRewriteModule, UrlRewritingNet.UrlRewriter"/>
    			...
    		</httpModules>
    	<system.web>
    	...
    	<system.webServer>
    		<modules>
    			<remove name="UrlRewriteModule"/>
    			<add name="UrlRewriteModule" type="UrlRewritingNet.Web.UrlRewriteModule, UrlRewritingNet.UrlRewriter"/>
    			...
    		</modules>
    	</system.webServer>

#### Forms

Umbraco Forms 6.0.0 已经发布，并兼容于 Umbraco 7.6.0，这是一个重要的发布版本严格依赖于 Umbraco 7.6.0。如果你要使用 Forms，你需要升级到 version 6.0.0


#### Courier

Umbraco Courier 3.1.0 已经发布，并兼容于 Umbraco 7.6.0。如果你要使用 Courier，你需要升级到 version 3.1.0