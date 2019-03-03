# 日期 #

`Returns: DateTime`

显示日历 UI，用于选择日期和时间并保存为 DateTime 值。

## 日期类型定义示例 ##

![Data Type Definition Example](images/DateTime-DataType.png)

唯一可用于操作DateTime 属性的设置只有格式设置。后台默认显示的格式未`YYYY-MM-DD`。但是你可以更改为其他的。在[MomentJS.com](https://momentjs.com/) 查看更多支持的格式。

## 内容示例 ##

![Content Example](images/Date-Time-Content.png)

## MVC 视图实例 - 显示日期 ##

### 类型 ###

	@(Model.Content.GetPropertyValue<DateTime>("datePicker").ToString("dd MM yyyy"))
	

### 动态 (Obsolete) ###

**警告**

**查看[常见陷阱](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics)来了解为什么动态方法已经过时的更多信息。**

```csharp
@{
    @CurrentPage.datePicker.ToString("dd-MM-yyyy")
}
```