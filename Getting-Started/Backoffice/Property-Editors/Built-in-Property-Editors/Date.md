# 日期

`返回值: DateTime`

显示一个日历 UI 用于选择日期，这个值保存的是标准的 datetime 值。

## 日期类型定义示例

![Data Type Definition Example](images/DateTime-DataType.png)

## 内容示例 

![Content Example](images/Date-Time-Content.png)

## MVC 视图实例 - 显示日期

###类型:

	@(Model.Content.GetPropertyValue<DateTime>("datePicker").ToString("dd MM yyyy"))

###动态: 

	@{
		@CurrentPage.datePicker.ToString("dd-MM-yyyy")
	}