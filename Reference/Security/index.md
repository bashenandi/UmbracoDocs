# 安全 #

_Umbraco 安全信息，它包含许许多多安全选项和配置项，用于任何和授权如何在 Umbraco 中工作_

## Umbraco 安全概述 ##

在我们的主站中有一个专用的安全页面，提供绝大多数你需要知道的关于安全使用 Umbraco 的相关信息，以及如何报告攻击：[https://umbraco.com/products/umbraco-cms/security/](https://umbraco.com/products/umbraco-cms/security/)

## SSL/HTTPS ##
我们强烈建议在 Umbraco 网站，尤其是生产环境中使用HTTPS。通过使用 HTTPS 你可以大大提高网站的安全性。

不要忘记 [使用 HTTPS 时如何配置Umbraco](use-https.md).

## 后台用户 ##
Umbraco 的后台用户授权使用的是[ASP.Net Identity](http://www.asp.net/identity)，这是一个非常灵活和易扩展的权限框架。

Umbraco 通过使用 Umbraco 的数据库数据封装了一个自定义 ASP.NET身份的实现。正常来说对于大多数 Umbraco 开发者是很好的，但是在有些场景中，权限处理需要进行定制化。ASP.NET 身份验证能够非常容易的通过使用自定义OAuth 提供者进行扩展，如果你想要你的用户通过自定义 OAuth 提供授权这是非常有用的，就像Azure活动目录一样，甚至Google 账户。
 
ASP.Net 身份验证对于你复写或者替换授权过程中的任何部分，都是足够灵活的。

### 自定义授权提供者 ###
Umbraco 后台为你的用户提供自定义 OAuth 授权支持。例如，任何 OpenID 链接提供者，例如 Azure 活动目录或身份验证服务器，Google，Facebook，Microsoft Account 等...

安装和配置一个自定义 OAuth 提供者你可以使用身份验证扩展包：[https://github.com/umbraco/UmbracoIdentityExtensions](https://github.com/umbraco/UmbracoIdentityExtensions)

The installation of these packages will install snippets of code with readme files on how to get up and running. Depending on the provider you've configured and its caption/color, the end result will look similar to:

![OAuth login screen](images/google-oauth.png)

#### Auto-linking accounts for custom OAuth providers

Traditionally a backoffice user will need to exist first and then that user can link their user account to an OAuth account in the backoffice, however in many cases the identity server you choose will be the source of truth for all of your users.

In this case you would want to be able to create user accounts in your identity server and then have that user given access to the backoffice without having to create the user in the backoffice first. This is done via auto-linking.

Read more about [auto linking](auto-linking.md)

### Custom password check