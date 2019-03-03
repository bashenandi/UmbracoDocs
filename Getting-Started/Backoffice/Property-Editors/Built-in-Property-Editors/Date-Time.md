# 日期时间 #

`Returns: DateTime`

显示日历 UI，用于选择日期和时间并保存为 DateTime 值。

## 日期时间定义类型示例 ##

![Data Type Definition Example](images/Date-Time-With-Time-Data-Type.png)

唯一可用于操作DateTime 属性的设置只有格式设置。后台默认显示的格式未`YYYY-MM-DD HH:mm:ss`。但是你可以更改为其他的。在[MomentJS.com](https://momentjs.com/) 查看更多支持的格式。

## 内容示例  ##

![Content Example](images/Date-Time-With-Time-Content.png)

## MVC 视图示例 - 显示包含时间的日期 ##

### 类型 ###

	@(Model.Content.GetPropertyValue<DateTime>("datePicker").ToString("dd MM yyyy HH:mm:ss"))

### 动态 (Obsolete) ###

**警告**

**查看[常见陷阱](https://our.umbraco.com/documentation/reference/Common-Pitfalls/#dynamics)来了解为什么动态方法已经过时的更多信息。**

```csharp
@{
    @CurrentPage.datePicker.ToString("dd-MM-yyyy HH:mm:ss")
}
```
