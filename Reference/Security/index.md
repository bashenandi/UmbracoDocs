# Security

_Umbraco 安全信息，它包含许许多多安全选项和配置项，用于任何和授权如何在 Umbraco 中工作_

## Umbraco 安全概述

在我们的主站中有一个专用的安全页面，提供绝大多数你需要知道的关于安全使用 Umbraco 的相关信息，以及如何报告攻击：[https://umbraco.com/products/umbraco-cms/security/](https://umbraco.com/products/umbraco-cms/security/)

## SSL/HTTPS
我们强烈建议在 Umbraco 网站，尤其是生产环境中使用HTTPS。通过使用 HTTPS 你可以大大提高网站的安全性。这有些使用 HTTPS 的好处：

* 信任 - 当你的网站部署在 HTTPS 环境下时，你的用户看到你的网站是安全的，他们可以浏览给你网站分配的证书，从而知道你的网站是合法的
* 减少网络攻击 ["Man in the middle"](https://www.owasp.org/index.php/Man-in-the-middle_attack) (或者网络劫持)
* Google 喜欢 HTTPS, 它会有利于你的网站排名

使用 HTTPS 的其他好处还有，如果你的服务器和浏览器支持，就可以使用[http2](https://en.wikipedia.org/wiki/HTTP/2)协议。

__UseSSL配置选项__
Umbraco 允许你为所有后台通信使用 SSL/HTTPS，使用下面的appSettings 配置：

	<add key="umbracoUseSSL" value="true" />

当这个选项设置为开启时，会做下面一些事情：

* 确保后台的授权 cookies 设置为[secure only](https://www.owasp.org/index.php/SecureFlag) (必须在 https 下传送)
* 所有非 https 的请求任何后台控制器，均重定向为 https
* 所有的内部分发Umbraco请求(例如. 计划发布，心跳包等等...)均执行在 https 下
* 所有的 Umbraco 通知 email 包含的链接均为 https 链接
* 所有后台处理程序和服务的授权请求如果不通过 https 均会被拒绝

一但你为你的网站启用了 HTTPS，所有的请求都会定向为 HTTPS 请求，这可以由 IIS 的重写规则来完成。为此需要安装 IIS 重写模块，多数主机提供商会默认启用。

在你的`web.config`中找到，或者添加`<system.webServer><rewrite><rules>` 部分，并将下面的规则添加进去。这个规则将所有`http://mysite.com`网站的请求重定向到安全的`https://mysite.com` 并且响应为永久定向状态。

	<rule name="HTTP to HTTPS redirect" stopProcessing="true">
		<match url="(.*)" />
		<conditions>
			<add input="{HTTPS}" pattern="off" ignoreCase="true" />
			<add input="{HTTP_HOST}" pattern="localhost" negate="true" />
		</conditions>
		<action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent" />
	</rule>        

注意这些规则包含了`localhost `忽略。如果你运行的本地环境不同于 URL`localhost`，你可以添加额外的忽略规则。此外，如果你有一个没有运行 HTTPS测试环境，你也可以添加到忽略规则里面。

## 后台用户

**适用于 version7.3.1以及更高**

Umbraco 的后台用户授权使用了[ASP.Net Identity](http://www.asp.net/identity)，这是一个非常灵活和易扩展的权限框架。

Umbraco 通过使用 Umbraco 的数据库数据封装了一个自定义 ASP.NET身份的实现。正常来说对于大多数 Umbraco 开发者是很好的，但是在有些场景中，权限处理需要进行定制化。ASP.NET 身份验证能够非常容易的通过使用自定义OAuth 提供者进行扩展，如果你想要你的用户通过自定义 OAuth 提供授权这是非常有用的，就像Azure活动目录一样，甚至Google 账户。
 
ASP.Net 身份验证对于你复写或者替换授权过程中的任何部分，都是足够灵活的。

### 自定义授权提供者
Umbraco 后台为你的用户提供自定义 OAuth 授权支持。例如，任何 OpenID 链接提供者，例如 Azure 活动目录或身份验证服务器，Google，Facebook，Microsoft Account 等...

安装和配置一个自定义 OAuth 提供者你可以使用身份验证扩展包：[https://github.com/umbraco/UmbracoIdentityExtensions](https://github.com/umbraco/UmbracoIdentityExtensions)

The installation of these packages will install snippets of code with readme files on how to get up and running. Depending on the provider you've configured and it's caption/color, the end result will look similar to:

![OAuth login screen](images/google-oauth.png)

#### 自动连接账户

Traditionally a back office user will need to exist first and then that user can link their user account to an OAuth account in the back office, however in many cases the identity server you choose will be the source of truth for all of your users. In this case you would want to be able to create user accounts in your identity server and then have that user given access to the back office without having to create the user in the back office first. This is done via auto-linking. There are auto-link options you can specify for your OAuth provider (see http://issues.umbraco.org/issue/U4-6753 for other details). 

Here's an example of specifying auto link options for your OAuth provider:

    //create the options, all parameters are optional but if you wish to enable
    //any auto-linking, the autoLinkExternalAccount parameter must be true
    var autoLinkOptions = new ExternalSignInAutoLinkOptions(
		autoLinkExternalAccount:true, 
		defaultUserType: "editor", 
		defaultCulture: "en-US");
    
    //an optional callback you can specify to give you more control over how the 
    //back office user is created (auto-linked)
    autoLinkOptions.OnAutoLinking = (BackOfficeIdentityUser user, ExternalLoginInfo info) =>
    {
		//this callback will execute when the user is being auto-linked but before it is created
		//so you can modify the user before it's persisted
    };
    
    identityServerOptions.SetExternalSignInAutoLinkOptions(autoLinkOptions);

### 替换基础的用户名/密码检查

Having the ability to simply replace the logic to validate a username and password against a custom data store is important to some developers. Normally in ASP.Net Identity this
would require you to override the `UmbracoBackOfficeUserManager.CheckPasswordAsync` implementation and then replace the `UmbracoBackOfficeUserManager` with your own class during startup. 
Since this is a common task we've made this process a lot easier with an interface called `IBackOfficeUserPasswordChecker`.

Here are the steps to specify your own logic for validating a username and password for the back office:

1. Install the UmbracoIdentityExtensions package https://github.com/umbraco/UmbracoIdentityExtensions 

1. Create an implementation of `Umbraco.Core.Security.IBackOfficeUserPasswordChecker`

	* There is one method in this interface: `Task<BackOfficeUserPasswordCheckerResult> CheckPasswordAsync(BackOfficeIdentityUser user, string password);`
	* The result of this method can be 3 things:
		* ValidCredentials = The credentials entered are valid and the authorization should proceed
		* InvalidCredentials = The credentials entered are not valid and the authorization process should return an error
		* FallbackToDefaultChecker = This is an optional result which can be used to fallback to Umbraco's default authorization process if the credentials could not be verified by your own custom implementation

	For example, to always allow login when the user enters the password `test` you could do:
	
		using System.Threading.Tasks;
		using Umbraco.Core.Models.Identity;
		using Umbraco.Core.Security;
		
		namespace MyNamespace
		{
		    public class MyPasswordChecker : IBackOfficeUserPasswordChecker
		    {
		        public Task<BackOfficeUserPasswordCheckerResult> CheckPasswordAsync(BackOfficeIdentityUser user, string password)
		        {
		            var result = (password == "test") 
		                ? Task.FromResult(BackOfficeUserPasswordCheckerResult.ValidCredentials)
		                : Task.FromResult(BackOfficeUserPasswordCheckerResult.InvalidCredentials);
		
		            return result;
		        }
		    }
		}

1. Modify the `~/App_Start/UmbracoCustomOwinStartup.cs` class

	* Replace the `app.ConfigureUserManagerForUmbracoBackOffice` call with a custom overload to specify your custom `IBackOfficeUserPasswordChecker`  

            var applicationContext = ApplicationContext.Current;
            app.ConfigureUserManagerForUmbracoBackOffice<BackOfficeUserManager, BackOfficeIdentityUser>(
                applicationContext,
                (options, context) =>
                {
                    var membershipProvider = Umbraco.Core.Security.MembershipProviderExtensions.GetUsersMembershipProvider().AsUmbracoMembershipProvider();
                    var userManager = BackOfficeUserManager.Create(options, 
                        applicationContext.Services.UserService,
                        applicationContext.Services.ExternalLoginService,
                        membershipProvider);
                    //Set your own custom IBackOfficeUserPasswordChecker   
                    userManager.BackOfficeUserPasswordChecker = new MyPasswordChecker();
                    return userManager;
                });	
                
1. Make sure to switch the `owin:appStartup` appSetting in your `web.config` file to use `UmbracoCustomOwinStartup`: `<add key="owin:appStartup" value="UmbracoCustomOwinStartup"/>`

**Note:** if the username entered in the login screen does not exist in Umbraco then `MyPasswordChecker()` does not run, instead Umbraco will immediately fall back to it's internal checks (default Umbraco behavior).

### 使用活动域认证授权

Umbraco 7.5.0+ comes with a built-in `IBackOfficeUserPasswordChecker` for Active Directory: `Umbraco.Core.Security.ActiveDirectoryBackOfficeUserPasswordChecker`. 

So like the above docs, instead of setting up your own `IBackOfficeUserPasswordChecker`, you can just use `Umbraco.Core.Security.ActiveDirectoryBackOfficeUserPasswordChecker` and then specify your AD domain in your web.config:

    <appSettings>
      	<add key="ActiveDirectoryDomain" value="mydomain.local" />
    </appSettings>

### [配置 Umbraco 兼容的FIPS 服务器](Setup-Umbraco-for-a-Fips-Server/index.md)

How to configure Umbraco to run on a FIPS compliant server.
