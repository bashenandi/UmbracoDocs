# Razor基本语法 #
_显示如何使用 Razor 的常见逻辑处理，类似 if/else,foreach 循环,switch 语句和使用 @ 符号分隔代码和标记。_

## @ 符号 ##
@符号用于 Razor 的初始代码中，告诉编辑器从哪里开始解释代码，而不只是将整个文件的内容作为文本返回。使用单个字符做这种分离，会使代码更加简洁、紧凑，更便于阅读。

	@*在 html 代码中写入值 *@

	<p>@Model.Content.Name</p>

	@*在属性内*@
	<a href="@Model.Content.Url">@Model.Content.Name</a>

	@*用作逻辑结构的开始*@
	@if(somethingIsTrue){
		<p>Write stuff</p>
	}

	@foreach(var item in Model.Content.Children){
		<li>@item.Name</li>
	}

## Razor中的嵌入式注释 ##
注释你的代码是非常重要的，用注释解释代码的作用。`@* *@`标示出了一段注释，它不会在输出结果中可见。

	@*Here we check if the name is equal to foobar*@
	@if(Model.Content.Name == "foobar"){
		@foreach(var child in Model.Content.Children){
			@* here we write stuff for each child page *@
			<p>write stuff</p>
		}
	}


## If/else ##
If/else 语句，当条件为真时执行一个任务，条件为非真时执行另一个任务

	@if(Model.Name == "home"){
		<p>This is the homepage!</p>
	}

	@if(Model.NodeTypeAlias == "TextPage"){
		<p>this is a textpage</p>
	}else{
		<p>this ia NOT a textpage</p>
	}

<<<<<<< HEAD
## Foreach 循环 ##
Foreach 通过条目的集合进行循环，通常是一个页面的集合，然后对每一项执行动作。

	@foreach(var item in Model.Content.Children){
		<p>The item name is: @Item.Name</p>
	}

## Switch 块 ##
当条件比较多的时候，可以使用 Switch 块进行判断

	@switch(Model.Content.WeekDay){
	case "Monday":
		"<p>It is Monday</p>;
		break;
	case "Tuesday":
		"<p>It is Tuesday</p>";
		break;
	case "Wednesday":
		"<p>It is Wednesday</p>";
		break;
	default:
		"<p>It's some day of the week</p>"
		break;
	}

###更多信息
- [更多例子](../../../Reference/Templating/Mvc/examples)
- [查询](../../../Reference/Querying)