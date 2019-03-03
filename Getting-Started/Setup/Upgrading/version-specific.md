# 特殊版本升级 #

*本文档涵盖了一些特定版本升级所需要的升级步骤，大多数版本不需要这些特定的升级步骤，多数时候你可以直接从当前版本升级到最新版本*

先按照常规升级指南中的步骤操作，然后再按照这些特殊版本的附加说明进行操作。 (请记住，如果没有提到新的配置文件，则表示他们已经在一般升级指南中描述过。)

## Version 7 to version 8 ##

目前没有直接升级的途径，但可以将内容从Umbraco 7站点迁移到Umbraco 8站点。我们正在开发一个内容迁移工具，使您能够将内容（内容/媒体/成员）从Umbraco 7站点移动到Umbraco 8站点。我们一准备好就通知你。

无法将Umbraco 7站点升级到Umbraco 8的原因，在于Umbraco 8中的代码库进行了功能上的更新。许多过时的代码和技术已经被删除，而新的、更快的和更安全的技术已经在整个Umbraco 8中实现。在与Umbraco7保持完全兼容的同时，根本不可能实现这一巨大飞跃。

在Umbraco 8中，我们添加了改进和更新了依赖项，并进行了彻底的清理，使您更容易处理和扩展Umbraco项目。

在不久的将来，虽然可以手动将您的Umbraco 7项目升级到Umbraco 8，当然我们会在准备好后立即向您提供最佳实践文档。我们强烈建议您等待升级Umbraco 7项目，直到我们准备好此文档，以确保您获得最佳的内容迁移体验。

## Version 7.7.0 ##

7.7.0版引入了用户组以及更好的用户管理和安全设施。这意味着与“User Type”有关的任何事情都不再存在，包括几个与User Type一起工作的API。如果您使用的代码或任何包的代码引用了“User Type”API，则可能需要对代码进行更改。在许多情况下，我们已经为这些场景创建了向后兼容性，并且废弃的API不应该再使用，但在某些情况下，这是不可能的。

此外，我们现在默认使用的是电子邮件地址，而不是用户名作为凭据。因此在尝试登录到BackOffice时，现在需要使用电子邮件地址，而不是以前版本中使用的用户名。如果您从旧版本进行升级，并且希望继续使用用户名，则需要将`<usernameIsEmail>true</usernameIsEmail>`设置更改为**false**。

查看完整的断崖式升级列表请前往: [the list on the issue tracker](http://issues.umbraco.org/issues/U4?q=Due+in+version%3A+7.7.0+Backwards+compatible%3F%3A+No+) 

版本7.7.2不再随`CookComputing.XmlRpcV2`程序集一起提供，因此，如果引用此程序集或具有需要此程序集的包，则可能需要在开始7.7.2升级之前从备份中将其复制回网站。

这个版本还提供了非常少的客户端文件（例如JS、CSS、图像），这些文件只与非常旧的Umbraco版本（即<7.0.0）相关。可能有一些包引用了这些旧的客户端文件，因此如果您发现缺少图像引用，可能需要与相关包的供应商联系以更新其引用。

## Version 7.6.3 ##
简述:

在 Umbraco 版本7.6.2，我们在 **Property Value Converts(PVCs)** 中犯了一个错误，在两天后的版本7.6.3中得到更正。如果你在前端查询中查询下列数据类型时遇到问题，请升级至 7.6.3：

 * Multi Node Tree Picker
 * Related Links
 * Member Picker

若果之前尝试修复了这些数据类型的问题，在升级到7.6.3之后您可能需要再次修复它们。

## 属性值转换器? ##

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

## Version 7.6.0 ##

7.6.0有了一些破坏性变化，请务必 **[在这里阅读了解](760-breaking-changes.md)** and [跟踪器中这些项目的列表](http://issues.umbraco.org/issues/U4?q=Due+in+version%3A+7.6.0+Backwards+compatible%3F%3A+No+)

三件需要注意的重要问题：

 1. 在 web.config 文件中，如果 `useLegacyEncoding` 当前设置为 `true` ，则 **不要** 设置为 `false` - 改变密码加密规则会让你再也无法登录。
 2. 在 umbracoSettings.config 文件中，保持 `EnablePropertyValueConverters` 设置为 `false` - 这会帮助你已有的内容选取器继续工作
 3. 在 tinyMceConfig.config 文件中，确保删除 `<plugin loadOnFrontend="true">umbracolink</plugin>` ，这样你的富文本编辑器就可以正常工作了


#### 通过 Nuget 更新 ####

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

#### Forms ####

Umbraco Forms 6.0.0 已经发布，并兼容于 Umbraco 7.6.0，这是一个重要的发布版本严格依赖于 Umbraco 7.6.0。如果你要使用 Forms，你需要升级到 version 6.0.0


#### Courier ####

Umbraco Courier 3.1.0 已经发布，并兼容于 Umbraco 7.6.0。如果你要使用 Courier，你需要升级到 version 3.1.0
## Version 7.4.0
For manual upgrades: 

* Copy the new folder `~/App_Plugins/ModelsBuilder` into the site
* Do not forget to merge `~/Config/trees.config` and `~/Config/Dashboard.config` - they contain new and updated entries that are required to be there
  * If you forget `trees.config` you will either not be able to browse the Developer section or you will be logged out immediately when trying to go to the developer section
* You may experience an error saying `Invalid object name 'umbracoUser'` - this can be fixed by [clearing your cookies on localhost](http://issues.umbraco.org/issue/U4-8031)

## Version 7.3.0
Make sure to manually clear your cookies after updating all the files, otherwise you might an error relating to `Umbraco.Core.Security.UmbracoBackOfficeIdentity.AddUserDataClaims()`. The error looks like: `Value cannot be null. Parameter name: value`.

NuGet will do the following for you but if you're upgrading manually:

* Delete `bin/Microsoft.Web.Helpers.dll`
* Delete `bin/Microsoft.Web.Mvc.FixedDisplayModes.dll`
* Delete `bin/System.Net.Http.dll`
* Delete `bin/System.Net.Http.*.dll` (all dll files starting with `System.Net.Http`) **EXCEPT** for `System.Net.Http.Formatting.dll`
* Delete `bin/umbraco.XmlSerializers.dll`
* In your `web.config` file, add this in the `appSetting` section: `<add key="owin:appStartup" value="UmbracoDefaultOwinStartup" />`

Other considerations:

* WebApi has been updated, normally you don’t have to do anything unless you have custom webapi configuration:
  * See this article if you are using `WebApiConfig.Register`: [https://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](https://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
  * You need to update your `web.config` file to have the correct WebApi version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
* MVC has been updated to MVC5
  * You need to update your `web.config` file to have the correct MVC version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
  * The upgrader will take care of updating all other web.config’s (in all other folders, for example, the `Views` and `App_Plugins` folders) to have the correct settings
  * For general ASP.NET MVC 5 upgrade details see: [https://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](https://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
* It is not required that you merge the changes for the Examine index paths in the ExamineIndex.config file. However, if you do, your indexes will be rebuilt on startup because Examine will detect that they don’t exist at the new location.
* It's highly recommended to clear browser cache - the ClientDependency version is automatically bumped during install which should force browser cache to refresh, however in some edge cases this might not be enough.

## Version 7.2.0
* Copy in the /Views/Partials/Grid (contains Grid rendering views)

## Version 7.1.0
* Remove the /Install folder.

## Version 7.0.1 to 7.0.2

* There was an update to the /umbraco/config/create/ui.xml which needs to be manually updated, the original element had this text:

		<nodeType alias="users">
			<header>User</header>
			<usercontrol>/create/simple.ascx</usercontrol>
			<tasks>
			  <create assembly="umbraco" type="userTasks" />
			  <delete assembly="umbraco" type="userTasks" />
			</tasks>
		</nodeType>

	* The &lt;usercontrol&gt; value has changed to: **/create/user.ascx**, this is a required change otherwise creating a new user will not work.
* There is a breaking change to be aware of, full details can be found [here](https://umbraco.com/blog/heads-up-breaking-change-coming-in-702-and-62/).

## Version 7.0.0 to 7.0.1
* Remove all uGoLive dlls from /bin
 * These are not compatible with V7
* Move appSettings/connectionStrings back to web.config
 * If you are on 7.0.0 you should migrate these settings into the web.config instead of having them in separate files in /config/
 * The keys in config/AppSettings.config need to be moved back to the web.config <appSettings> section and similarly, the config/ConnectionStrings.config holds the Umbraco database connections in v7.0.0 and they should be moved back to the web.config <connectionStrings> section.
 * /config/AppSettings.config and /config/ConnectionString.config can be removed after the contents have been moved back to web.config. (Make backups just in case)
* Delete all files in ~/App_Data/TEMP/Razor/*
 * Related to issues with razor macros

## Version 6 to 7.0.0
Read and follow [the full v7 upgrade guide](upgrading-to-v7.md)

## Version 4.10.x/4.11.x to 6.0.0
* If your site was ever a version between 4.10.0 and 4.11.4 and you have just upgraded to 6.0.0 install the [fixup package](https://our.umbraco.com/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.
* The DocType Mixins package is **NOT** compatible with v6+ and will cause problems in your document types.

## Version 4.10.x to 4.11.x
* If your site was ever a version between 4.10.0 and 4.11.4 install the [fixup package](https://our.umbraco.com/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.

## Version 4.8.0 to 4.10.0
* Delete the bin/umbraco.linq.core.dll file
* Copy the new files and folders from the zip file into your site's folder
 * /App_Plugins
 * /Views
 * global.asax
* Remove the Config/formHandlers.config file

## Version 4.7.2 to 4.8.0
* Delete the bin/App_Browsers.dll file
* Delete the bin/App_global.asax.dll file
* Delete the bin/Fizzler.Systems.HtmlAgilityPack.dll file
* For people using uComponents 3.1.2 or below, 4.8.0 breaks support for it. Either upgrade to a newer version beforehand or follow the workaround [posted here](https://our.umbraco.com/projects/backoffice-extensions/ucomponents/questionssuggestions/33021-Upgrading-to-Umbraco-48-breaks-support-for-uComponents)

## Version 4.7.1.1 to 4.7.2
* Delete the bin/umbraco.MacroEngines.Legacy.dll file

## Version 4.6.1 to 4.7.1.1
* Delete bin/Iron*.dll (all dll files starting with "Iron")
* Delete bin/RazorEngine*.dll (all dll files starting with "RazorEngine")
* Delete bin/umbraco.MacroEngines.Legacy.dll
* Delete bin/Microsoft.Scripting.Debugging.dll
* Delete bin/Microsoft.Dynamic.dll
