# 教程 - 给属性编辑器添加服务端代码 #

## 概述 ##
在这篇指南中我们会添加一个服务端的 API 控制器，它会从 Umbraco 数据中查询一个自定义的表，并且返回一些数据给到一个简单的 angular 控制器和视图。

最终结果会得到一个使用自定义表中的数据填充的人员列表。当点击它时，会存储选中人员的ID。

## 设置数据库 ##
首先我们需要一些数据；下面是一个简单的 SQL 脚本，用于创建一个包含一些随机数据的`people`表。你也可以使用[http://generatedata.com](http://generatedata.com)生成海量的数据：

	CREATE TABLE people (
		id INTEGER NOT NULL IDENTITY(1, 1),
		name VARCHAR(255) NULL,
		town VARCHAR(255) NULL,
		country VARCHAR(100) NULL,
		PRIMARY KEY (id)
	);
	GO

	INSERT INTO people(name,town,country) VALUES('Myles A. Pearson','Tailles','United Kingdom');
	INSERT INTO people(name,town,country) VALUES('Cora Y. Kelly','Froidchapelle','Latvia');
	INSERT INTO people(name,town,country) VALUES('Brooke Baxter','Mogi das Cruzes','Grenada');
	INSERT INTO people(name,town,country) VALUES('Illiana T. Strong','Bevel','Bhutan');
	INSERT INTO people(name,town,country) VALUES('Kaye Frederick','Rothesay','Turkmenistan');
	INSERT INTO people(name,town,country) VALUES('Erasmus Camacho','Sint-Pieters-Kapelle','Saint Vincent and The Grenadines');
	INSERT INTO people(name,town,country) VALUES('Aimee Sampson','Hawera','Antigua and Barbuda');
	

## 设置 ApiController 路由 ##
接下来我们需要定义一个`ApiController`来暴露我们应用的服务端路由，用于填充数据。

为此，我们要创建`/App_Code/PersonApiController.cs`文件。如果想在应用启动时编译它，我们必须把她放在`App_Code`文件夹中。一个替代方案，你也可以把它添加到正常的.NET 项目中，编译为 DLL 后使用。

在`PersonApiController.cs`文件中, 添加： 

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Web;

	using Umbraco.Web.WebApi;
	using Umbraco.Web.Editors;
	using Umbraco.Core.Persistence;

	namespace My.Controllers
	{
	    [Umbraco.Web.Mvc.PluginController("My")]
	    public class PersonApiController : UmbracoAuthorizedJsonController
	    {
	        // we will add a method here later
	    }
	}

这是一个非常基础的API 控制器，继承自`UmbracoAuthorizedJsonController`，这是一个特殊类，仅返回 JSON 数据，并且仅响应通过了后台授权的请求。

## 设置 GetAll() 方法 ##

现在我们有了一个控制器，我们还需要创建一个方法，可以返回人员的集合，以便我们的编辑器使用。

所以首先我们在`My.Controllers`命名空间中创建一个`Person `类：

	public class Person
	{
	    public int Id { get; set; }
	    public string Name { get; set; }
	    public string Town { get; set; }
	    public string Country { get; set; }
	}

我们使用这个类将表里的数据映射为 C#的类，稍后我们就可以返回JSON 了。

现在我们需要一个方法用来返回`people `的集合，添加下面的代码到`PersonApiController `类中：

	public IEnumerable<Person> GetAll()
	{
		
	}

在`GetAll ()`方法中，我们现在加入一些少量的代码，用于连接到数据库，创建查询语句并且返回数据，再映射到上面的`Person`类：
	
	/* 获取数据库连接 */
	var db = UmbracoContext.Application.DatabaseContext.Database;
	
	/* 创建查询从 people 表中获取所有内容 */
	var query = new Sql().Select("*").From("people");
	
	/* 通过查询从 DB 中获取数据，并映射为 Person 对象 */
	return db.Fetch<Person>(query);

现在我们完成了服务端的工作，你可以通过`/umbraco/backoffice/My/PersonApi/GetAll`访问这个文件。注意必须是登录后台之后。

这将向我们返回 JSON 数据。（错误！实际这里不能通过 url 访问，会返回407错误）

## 创建 Person 资源 ##

Now that we have the server-side in place, and a URL to call, we will setup a service to retrieve our data. As an Umbraco-specific convention, we call these services a *resource*, so we always have an indication of what services fetch data from the DB.

现在服务器端已经就位了，还有一个可调用的 URL，我们还要配置一个服务来检索这些数据。作为Umbraco 特有的惯例，我们叫这些服务为*resource*，所以我们始终有一个从数据库中获取数据的服务标识。


创建一个新的 js 文件`person.resource.js`，贴入下面的代码：

	/* 添加资源到 umbraco.resources 模块*/
	angular.module('umbraco.resources').factory('personResource', 
	function($q, $http, umbRequestHelper){
		/* 工厂对象返回 */
		return {
			/* 这里调用我们之前创建的ApiController */
			getAll: function(){
				return umbRequestHelper.resourcePromise($http.get("backoffice/My/PersonApi/GetAll"), "Failed to retrieve all Person data");
			}
		}
	});

这里使用的是标准的 angular 工厂模式，因此我们现在可以注入`personResource`下面的方法到任何的控制器中。

`getAll()`方法从`$http.get`调用中返回一个promise，用来控制调用的 URL，当其准备好时返回数据。你应该注意到`$http.get`方法包含在`umbRequestHelper.resourcePromise`中，后者会自动处理任何500错误，这就是第二个参数的用处 - 它定义了发生错误时显示的信息。

## 创建视图和控制器 ##

现在，我们将最终设置一个新的视图和控制器，它遵循以前的教程，因此您可以参考这些教程了解更多详细信息：

#### 视图 ####

	<div ng-controller="My.PersonPickerController">
		<ul>
			<li ng-repeat="person in people">
				<a href ng-click="model.value = person.Name">{{person.Name}}</a>
			</li>
		</ul>
	</div>
	
#### 控制器 ####

	angular.module("umbraco").controller("My.PersonPickerController", 
	function($scope, personResource){
		personResource.getAll().then(function(response){
			$scope.people = response.data;
		});
	});

## 工作流 ##
在所有这些都做完之后，你需要将其注册到属性编辑器的包装清单中 - 请看这个系列的第一篇教程。你需要告诉包在 app 启动的时候加载`personpicker.controller.js`和`person.resource.js`文件。

有了这个，整体的流程就是：

1. 视图用控制器返回的数据输出 people 列表
2. 控制器请求personResource 获取数据
3. personResource返回一个Promise，向my/PersonAPI ApiController请求数据
4. ApiController查询数据库，返回Person强类型数据对象
5. ApiController 已 JSON 格式返回`Person`对象 给前端资源
6. 前端资源解析Promise
7. 控制器填充视图

容易吗？- 老实说，这里有很多东西需要持续跟踪，但是每个组件都很小巧并且很灵活。


## 总结 ##
上面最重要的部分是创建`ApiController`调用数据库获取你自己的数据，最终通过`$http`服务暴露这些数据给 angular。

为了简化，你也可以跳过service部分，直接在控制器中调用`$http`查询数据，但是通过 service来获取数据，可以在整个应用中多次重用资源。