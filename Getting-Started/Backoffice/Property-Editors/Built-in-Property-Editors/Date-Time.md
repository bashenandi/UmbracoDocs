# 日期时间

`返回值: DateTime`

显示日历 UI，用于选择日期和时间，这个值保存的是标准的 datetime 值

## 日期时间定义类型示例

![Data Type Definition Example](images/Date-Time-With-Time-Data-Type.png)

## 内容示例 

![Content Example](images/Date-Time-With-Time-Content.png)

##MVC 视图示例 - 显示包含时间的日期 

###类型:

	@(Model.Content.GetPropertyValue<DateTime>("datePicker").ToString("dd MM yyyy HH:mm:ss"))

###动态: 

	@{
		@CurrentPage.datePicker.ToString("dd-MM-yyyy HH:mm:ss")
	}