# Umbraco 7.6 断崖式升级 #

## 依赖

### UrlRewriting.Net ([U4-9004](https://issues.umbraco.org/issue/U4-9004))

UrlRewriting 是陈旧的，会导致内存泄露，当规则较多时会减慢网站启动数独。它完全被 [IIS Url Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) 扩展所替代。

### Json.Net ([U4-9499](https://issues.umbraco.org/issue/U4-9499))

Json.Net 更新到 version 10.0.0，得益于功能的改进，性能得到改进 (see [发行说明](https://github.com/JamesNK/Newtonsoft.Json/releases))。这*可能*对依赖于其功能的改变，出现一些破坏性改变。

### Log4net ([U4-1324](https://issues.umbraco.org/issue/U4-1324))

由于历史原因，直到 version7.5 ，Umbraco 还在使用基于旧版本(1.2.11)的自定义构建log4net，仅支持中等信任。然而，Umbraco 自身已经不再支持中等信任，因此 log4net 也随之升级为标准版本，最新构建版本为 log4net 2.0.8。

### ImageProcessor ([U4-8963](https://issues.umbraco.org/issue/U4-8963))

为了支持后台颜色参数，为`GetCropUrl`方法增加了一个可选参数。从而破坏了方法签名，可能需要重新编译用户代码。

### HtmlAgilityPack ([U4-9655](https://issues.umbraco.org/issue/U4-9655))

HtmlAgilityPack 更新到了 version 1.4.9.5。Umbraco 升级过程应该照顾到了适当地重定向绑定。

## 内核

### Membership Provider Encoding ([U4-6566](https://issues.umbraco.org/issue/U4-6566))

Membership provider，默认设置 `useLegacyEncoding` 为 `false` ，这是因为传统的密码编码有弱点。

这个改动仅影响新的安装（升级不会导致变化）。

### Property Value Converters ([U4-7318](https://issues.umbraco.org/issue/U4-7318))

由社区贡献的大量属性值转换器，已经合并成为默认的值转换器。这些转换器，将`GetPropertyValue`默认返回的 object 转换为了更多的类型。

例如，`SliderValueConverter` 可以返回 `decimal` 或者 `Range<decimal>` 直接在 views 中使用，而不是之前返回的 CSV字符串值。

这个改动仅影响新的安装（升级不会导致变化）。

新的属性值转换器在`umbracoSetting.config`中设置: 在节点`settings/content`中, 设置 `EnablePropertyValueConverters` 为 `true` 来进行激活。

### Database ([U4-9201](https://issues.umbraco.org/issue/U4-9201))

虽然 Umbraco 从 version 7以来一直使用 PetaPoco-managed 管理`UmbracoDatabase`实例，我们意识到一些遗留代码可以绕过机制和使用并行，脱离数据库链接，导致一些事务问题。

旧的代码已经依赖`UmbracoDatabase`实例进行了重构。然而，由于数据库会在`EndRequest`期间释放, 代码在其后运行则无法再进行释放，因此应该更新为在`EndRequest`之前使用任何一个 `HttpModule` 事件，或者创建一个新的`UmbracoModule.EndRequest` 事件。

更多问题可以在这里找到 [issue 146](https://github.com/kipusoep/UrlTracker/issues/146) 

### Scopes ([U4-9406](https://issues.umbraco.org/issue/U4-9406))

Version 7.6 introduces the notion of *scopes*, which allow for wrapping multiple service-level operations in one single transaction. Although for various reasons the scopes API is partially public, scopes are not meant for public use at this stage and we need a few more releases to ensure that the APIs are stable.

Scopes *should not* change how Umbraco is functioning.

Introducing scopes means that some public APIs signatures are changing. Most of these changes target internal and/or non-breaking APIs (as per our [guidelines](https://our.umbraco.com/Documentation/Development-Guidelines/breaking-changes)) and therefore should have no impact on sites&mdash;but may break unit tests.

### Property Editors storing UDI instead of ID ([U4-9310](https://issues.umbraco.org/issue/U4-9310))

The property editors for pickers for content, media, members and related links have been updated to store UDI instead of node ID. Pickers in sites being upgraded have been marked as obsolete, but will continue to work as they always did.

New sites will have the obsolete pickers filtered out from the list of available property editors, but they can be enabled by a configuration flag.

### RTE Images attributes ([U4-6228](https://issues.umbraco.org/issue/U4-6228), [U4-6595](http://issues.umbraco.org/issue/U4-6595))

For a long time we had a "rel" attribute on an "img" tag when inserting into the RTE. This is invalid HTML markup. We worked around this by stripping this attribute using a Property Editor Value converter. Some developers relied on this attribute unfortunately so we didn't just change it to a "data-id" attribute which would have been valid. In 7.6 we are not storing INT ids in these attributes and instead storing UDI values so with this change we no longer use "rel" or "data-id" and instead there will be a "data-udi" attribute. This change should affect only a very small amount of people that were previously relying on the values from the "rel" attribute.

### Others

我们在内核使用了 SignalR version 2.2.1, 如果你已经在你的 app 使用了版本的SignalR，可能会导致一些冲突。

version 7.6.0不在支持创建和修改 WebForms 模板。
