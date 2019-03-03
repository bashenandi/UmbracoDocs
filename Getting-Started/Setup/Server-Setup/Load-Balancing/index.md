# 负载均衡环境中运行Umbraco #

_有关如何将 Umbraco部署到负载均衡环境的方案以及其他细节情况。_

## 概述 ##

配置安装负载均衡环境，需要做好规划、设计和测试。这篇文档会协助你设置你的服务器，负载均衡设置以及 Umbraco 的配置。

本文档假设你已经掌握了如下的知识:

* Umbraco
* IIS 7+
* Networking & DNS
* Windows Server (2008/2012)
* .NET Framework

__强烈建议你安装和生产环境一样的测试负载均衡环境，这样你就可以进行完全一致的测试之后再更新至生产环境。__

## 灵活的负载均衡设置 ##

从version 7.3.0开始, 负载均衡的配置比以前任何版本都要简单。建立一个负载均衡环境是非常简单的，需要做的只是几个配置文件的变更。

[完整文档在这里](flexible.md)  

## 传统负载均衡设置 ##

Umbraco versions 7.3.0 之前必须使用传统又过时的负载均衡技术。

[完整文档在这里](traditional.md)  

## 通用负载均衡设置信息 ##

_下面的部分对于所有的负载均衡方式的配置都是通用的，不管你选择了哪种方案，请确保按照以下的过程进行操作._

### ASP.NET 配置 ###

* 你需要使用自定义的机器密钥，以保证所有服务器上的机器密钥加密值是一样的，否则你会看到视图状态错误而导致中止，验证错误和加密/解密错误基本都是由于服务器使用了其自己生成的密钥
	* 这里有两个工具可以用于生成机器密钥：
		* 	[http://www.betterbuilt.com/machinekey/](http://www.betterbuilt.com/machinekey/)
		* 	[http://www.developerfusion.com/tools/generatemachinekey/](http://www.developerfusion.com/tools/generatemachinekey/)
	* 	然后你需要更新你的 web.config 文件，注意  validation/decryption 类型可能会与你生成的不同。

			<configuration>
				<system.web>
					<machineKey decryptionKey="Your decryption key here" 
							validationKey="Your Validation key here" 
							validation="SHA1" 
							decryption="AES" />
				</system.web>
			</configuration>
			
	* 如果在你的应用中使用了 SessionState , 或者在 MVC 中使用了默认的TempDataProvider（使用了会话状态），你需要配置你的应用程序使用 SqlSessionStateStore provider (访问 [http://msdn.microsoft.com/en-us/library/aa478952.aspx](http://msdn.microsoft.com/en-us/library/aa478952.aspx) 了解更多)。


### 日志 ###

无论你选择了哪种负载均衡方案，都需要考虑一些日志的配置信息。

[完整文档在这里](logging.md)

### 测试 ###

你的测试环境应该也是同样的负载均衡环境，以便在更新到生产环境之前在同样的环境中发现任何问题。确定所有问题的最好方法是让很多人参与测试这些设置，并且确保应用程序日志及系统日志中的所有问题和警告都得到修复。

You'll need to test this solution **a lot** before going to production. You need to ensure there are no windows security issues, etc... The best way to determine issues is have a lot of people testing this setup and ensuring all errors and warnings in your application/system logs in Windows are fixed.

Ensure to analyze logs from all servers and check for any warnings and errors.

## FAQs ##

_下面是些关于 Umbraco 负载均衡的常见问题:_

__Question>__ _为什么我需要一个单独的实例用于 Umbraco 管理?_

_TL:DR_ 你不能将 Umbraco 后台管理部署在负载均衡中，否则会导致数据完整性或数据脏读。


你需要独立管理服务器的原因，在于没有办法（目前）使用数据库级别的锁在服务器之间保证数据的一致性，我们只能使用应用程序级别的锁来保证数据的一致性，而它只能运行在单一的服务器上。

如果你有多个管理员同时保存和发布，在不同服务器之间同时进行数据的读取和写入，会导致数据脏读，而数据库必须是一致的，否则最终会导致数据损坏。此外，缓存写入的结构表对于负载均衡来说非常重要，而其必须有一台独立的服务器进行读取。

__Question>__ _我的主管理服务器是否可以提供前端访问?_

可以。你的主管理服务器，同时提供前端访问是没有问题的。但是，如果您希望为前端服务器和后台设置不同的安全策略，您可以选择不这样做。

__Question>__ _我可以在负载均衡环境中使用 Courier 麽？?_

可以! 但是你必须要注意的是当你通过 Courier 推送你的更改到负载均衡时，你要配置Courier 仅推送到主管理服务器。如果你推送内容或者媒体文件的更改，你必须确保你所有环境中的架构元素（DocumentType 或者 MediaType 等）是完全一致的，否则你会遇到错误。当然，你也可以通过 Courier 在不同环境见推送这些架构修改。

## 更多信息
- Codegarden '15 session: [Umbraco Load Balancing](https://vimeo.com/132815038)
