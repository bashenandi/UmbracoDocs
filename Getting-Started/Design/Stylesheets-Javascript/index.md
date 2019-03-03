# 使用样式表和脚本 #

## 打包 &amp; 压缩 JS 和 CSS ##

你当然可以使用任何你喜欢的工具来打包 &amp; 压缩，但是 Umbraco 自带的ClientDependency框架不需要额外做什么事情就可以实现运行时打包 &amp; 压缩。

你可以在视图文件中像下面一样打包 &amp; 压缩。

	@using ClientDependency.Core.Mvc
	@using ClientDependency.Core
	@{
		Html.RequiresJs("~/scripts/Script1.js", 1);
		Html.RequiresJs("~/scripts/Script2.js", 2);
		
		Html.RequiresCss("~/css/style.css");
	}
	<html>
	<head>
		@Html.RenderCssHere()
		@Html.RenderJsHere()
	</head>

ClientDependency框架的详细信息可以在这里找到：[http://github.com/Shandem/ClientDependency](http://github.com/Shandem/ClientDependency)