# 后台身份验证路由需求 #

为了让Umbraco对后台的请求进行身份验证，需要指定路由。任何 URL 都路由至：

> /umbraco/backoffice/*

进行身份验证。如果您有一个不在前缀中路由的控制器，它将不会被后台认证使用。

如果使用WebApi 并使用`Umbraco.Web.WebApi.UmbracoAuthorizedApiController`（或任何继承的控制器），都无需担心路由问题，因为它们是自动路由的。所有`UmbracoAuthorizedApiController`（也包括`UmbracoAuthorizedJsonController`）的实现都会由默认路由来进行路由：

> `/umbraco/backoffice/api/{controller}/{action}`

在某些案例中，Umbraco Api 控制器是基于一个'插件的控制器'，那么它的路由为：

> `/umbraco/backoffice/{pluginname}/{controller}/{action}`

_注意:_ {area}是由[PluginController]属性指定的，用来替代/api/ 区域的路由。


## 后台MVC控制器 ##

如果你在后端使用 MVC，一般来说会从`Umbraco.Web.Mvc.UmbracoAuthorizedController`继承。这个类型的控制器并不会像 Umbraco Api 控制器那样自动路由，因此需要定义一个自定义路由使其工作。

有关认证/授权控制器和属性的更多信息，请参阅[控制器文档](../../../Implementation/Controllers/index.md)。

## 定义一个路由 ##
当你创建一个继承自`Umbraco.Web.Mvc.UmbracoAuthorizedController`的控制器时，你需要明确地定义一个路由。定义路由是由标准的ASP.NET MVC路由实现来完成的。在 Umbraco 中，你一般在`Umbraco.Core.ApplicationEventHandler.ApplicationStarted`事件中创建路由，像下面一样：


    protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext    
    applicationContext)
    {
      RouteTable.Routes.MapRoute(
      name: "cats",
      url: GlobalSettings.UmbracoMvcArea + "/backoffice/cats/{action}/{id}",
      defaults: new
      {
        controller = "Cats",
        action = "Meow",
        id = UrlParameter.Optional
      });
    }

_注意 路由必须使用`GlobalSettings.UmbracoMvcArea`配置并解析的 Umbraco 路径作为前缀，然后为了使 Umbraco 校验用户权限需要添加 "backoffice"。 _

### Surface 控制器是什么? ###
Surface 控制器不能在后台使用。Surface 并不是为了在后台工作而设计的，它们不打算在那里使用，也不支持在那里使用。

## 用于用户身份验证的特殊后台路由 ##
有一些特殊的路由会被Umbraco用来检查身份验证是用户的成员。

如果任何路由在路径中包含类似`.aspx`的扩展名，或以下路径始终是后台路由：

*  /Umbraco/RestServices
*  /Umbraco/BackOffice

如果路线不是上述任何一条，并且没有扩展，那么Umbraco无法确定它是后台还是前端 —— 因此会假设为前端。如果使用的是`UmbracoApiController`而不是`UmbracoAuthorizedApiController`，并且未使用`[isbackoffice]`属性，则会发生这种情况。

在上述所有情况下，都将使用用户身份验证。
