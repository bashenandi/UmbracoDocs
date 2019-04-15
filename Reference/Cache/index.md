# 缓存和分布式缓存 #

_本章介绍了如何以一致的方式在 Umbraco 应用程序中实现缓存功能，这在单服务器环境和负载均衡（多服务器）环境中都适用。本节中描述的缓存内容仅与 web 应用程序上下文中的应用缓存相关。_



## 如果你正在使用缓存，请阅读这里 ##

Although caching is a pretty standard concept it is very important to make sure that caching is done correctly and consistently. It is always best to ensure performance is at its best before applying any cache and also beware of *over caching* as this can cause degraded performance in your application because of cache turnover.

In normal environments caching seems to be a pretty standard and easy concept, however if you are a package developer or a developer who is going to be publishing a codebase to a load balanced environment then you need to be aware of how to invalidate your cache properly so that it works in load balanced environments. If it is not done correctly then your package and/or codebase will not work the way that you would expect in a load balanced scenario. 


**如果你缓存了业务逻辑数据，并且修改是基于用户在后台中进行，同时没有使用 *ICacheRefresher*，那么你需要根据下面的文档，重新检查你的代码。 **

## 检索并添加项目到缓存中 ##

你可以阅读[更新、新增项目到缓存中](updating-cache.md)。


## 刷新/废弃 缓存

### [ICacheRefresher](cache-refresher.md)

在 Umbraco 中作废缓存的标准方法是实现`ICacheRefresher`。

该接口包含了下面的方法：

* `Guid UniqueIdentifier { get; }` - 返回唯一的GUID 标识
* `string Name { get; }` - 缓存器的名字 (易于识别)
* `void RefreshAll();` - 废弃或者刷新所有使用`ICacheRefresher`创建的缓存。 例如，你缓存了`Employee`对象，这个方法将会废弃所有的`Employee`缓存。
* `void Refresh(int Id);` - 废弃或刷新根据提供的 INT 类型 id 所得到的对象。
* `void Refresh(Guid Id);` - 废弃或刷新根据提供的 GUID 类型 id 所得到的对象。
* `void Remove(int Id);` -  废弃根据提供的 INT 类型 id 所得到的对象。在多数情况下 Remove 和 Refresh 表现为同样的操作，但是在一些情况下`Refresh`不仅是删除/废弃一个缓存实例，还会更新它。 `Remove`明确的用于删除/废弃一个缓存实例。

_其中一些方法可能与您自己的缓存释放需求不相关，因此并非所有方法都需要执行逻辑_

`ICacheRefresher`还有两种基本类型，分别是：

* [`ICacheRefresher<T>`](cache-refresher-t.md) - 继承自`ICacheRefresher`并且提供了一个强类型方法用于释放缓存。这在执行调用缓存刷新器的方法时很有用，因为这样可以避免再次检索对象的开销。
  * `void Refresh(T instance);` - this would invalidate/refresh a single cache for the specified object.
  * `void Remove(T instance);` - this would invalidate a single cache for the specified object.
* `IJsonCacheRefresher` - this inherits from `ICacheRefresher` but provides more flexibility if you need to invalidate cache based on more complex scenarios (e.g. the [MemberGroupCacheRefresher](https://github.com/umbraco/Umbraco-CMS/blob/dev-v7/src/Umbraco.Web/Cache/MemberGroupCacheRefresher.cs)).
  * `void Refresh(string jsonPayload)` - Invalidates/refreshes any cache based on the information provided in the JSON. The JSON value is any value that is used when executing the method to invoke the cache refresher.

There are several examples of `ICacheRefresher`'s in the core: https://github.com/umbraco/Umbraco-CMS/tree/dev-v7/src/Umbraco.Web/Cache

### Executing an ICacheRefresher

To execute your `ICacheRefresher` you call these methods on the `DistributedCache.Instance` instance (the `DistributedCache` object exists in the `Umbraco.Web.Cache` namespace):

* `void Refresh<T>(Guid cacheRefresherId, Func<T, int> getNumericId, params T[] instances)` - this executes an `ICacheRefresher<T>.Refresh(T instance)` of the specified cache refresher Id
* `void Refresh(Guid cacheRefresherId, int id)` - this executes an `ICacheRefresher.Refresh(int id)` of the specified cache refresher Id
* `void Refresh(Guid cacheRefresherId, guid id)` - this executes an `ICacheRefresher.Refresh(int id)` of the specified cache refresher Id
* `void Remove(Guid factoryGuid, int id)` - this executes an `ICacheRefresher.Remove(int id)` of the specified cache refresher Id
* `void Remove<T>(Guid factoryGuid, Func<T, int> getNumericId, params T[] instances)` - this executes an `ICacheRefresher<T>.Remove(T instance)` of the specified cache refresher Id
* `void RefreshAll(Guid factoryGuid)` - this executes an `ICacheRefresher.RefreshAll()` of the specified cache refresher Id
* `void RefreshByJson(Guid factoryGuid, string jsonPayload)` - this executes an `IJsonCacheRefresher.Refresh(string jsonPayload)` of the specified cache refresher Id

**So when do you use these methods to invalidate your cache?**

This really comes down to what you are caching and when it needs to be invalidated. _An example:_ In your web application you are using MVC Donut Caching and you want to make sure this cache is invalidated whenever content in Umbraco is published. To do this, you could create an `ICacheRefresher` and leave the logic for `Remove`, `Refresh` empty since it's not needed in this case. In the `RefreshAll` method, add the logic to clear the Donut Cache. Then add an event handler for `ContentService.Published`, check if there is anything being published and if so execute `DistributedCache.Instance.RefreshAll([The GUID of your ICacheRefresher]);`.

### What happens when an ICacheRefresher is executed?

When an `ICacheRefresher` is executed via the `DistributedCache.Instance` a notification is sent out to all servers that are hosting your web application to execute the specified cache refresher. When not load balancing, this simply means that the single server hosting your web application executes the `ICacheRefresher` directly however when load balancing, this means that Umbraco will ensure that each server hosting your web application executes the `ICacheRefresher` so that each server's cache stays in sync.

## Events handling to refresh cache

To use the extensions add a using to `Umbraco.Web.Cache`;  You can then invoke them on the DistributedCache.Instance object.

## IServerMessenger

The server messenger broadcasts 'distributed cache notifications' to each server in the load balanced environment.
The server messenger ensures that the notification is processed on the local environment.

## [ApplicationContext.Current.ApplicationCache](applicationcache.md)

ApplicationCache is a container for the different cache types

