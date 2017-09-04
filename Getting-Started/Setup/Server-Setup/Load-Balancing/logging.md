# 负载均衡中的日志

自从Umbraco 使用 log4net 作为日志记录，有了一些特殊的配置，在确保日志正常运行的前提下，可以使用你喜欢的方式。如果你的基于文件方式记录，你应该确保你的日志文件名包含了机器名，否则你获取的文件会锁定。*(见下文详细说明如何解决这个问题)*

其他选择，包括改变你的 log4net设置，将日志记录到中央数据库-当然如果你没有数据操作权限则无法记录，你要意识到这一点。

## 包含机器名的Log4net日志文件

这些描述告诉你如何配置 log4net，在写入日志文件时，包含机器名。

简单的更新你的 log4net配置文件 appender 值如下：

	<appender ....>
		<!--
			THIS IS THAT VALUE THAT UMBRACO IS SHIPPED WITH THAT DOES NOT INCLUDE THE MACHINE NAME IN THE FILE
			<file value="App_Data\Logs\UmbracoTraceLog.txt" />
		-->
		
		<!-- THIS IS THE NEW CHANGE TO HAVE A MACHINE NAME IN THE FILE NAME -->
		<file type="log4net.Util.PatternString" 
			value="App_Data\Logs\UmbracoTraceLog%property{log4net:HostName}.txt" />
	</appender>