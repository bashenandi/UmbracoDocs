#创建表单
创建表单需要.NET MVC 的知识体系。因此如果你熟悉了添加视图模型，视图和控制器，你已经准备好创建你的第一个表单了。

*你也可以使用[Umbraco forms](https://umbraco.com/products/umbraco-forms/)。它可以使你和你的编辑人员在后台创建和处理表单。 这包含了验证，跳转和存储和发送表单数据。优秀的 UI，易于扩展并且被 Umbraco HQ 所支持。*

在这个示例中，我们会创建一个基本的联系表单包含：name，email 和message 字段。
In this example we'll create a basic contact form contain name, email and message field.

###创建视图模型
首先我们通过添加一个新类到`/Models`来为联系表单创建模型。我们称它为： `ContactFormViewModel.cs`
	
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;

    namespace MyFirstForm.Models
    {
        public class ContactFormViewModel {
            public string Name { get; set; }
            public string Email { get; set; }
            public string Message { get; set; }
        }
    }

在添加模型后生成你的解决方案。

###创建视图
接下来我们为表单添加一个视图到`/View/Partials`目录。因为我们已经添加了模型并构建了解决方案，所以我们可以将它添加为强类型视图。

视图可以使用标准的 MVC helper 进行构建：

    @model MyFirstForm.Models.ContactFormViewModel

    @using(Html.BeginUmbracoForm("Submit", "ContactForm")) {
        <div>
            @Html.TextBoxFor(m=>m.Name)
        </div>
        <div>
            @Html.TextBoxFor(m=>m.Email)
        </div>
        <div>
            @Html.TextAreaFor(m=>m.Message)
        </div>
        <input type="submit" name="Submit" value="Submit" />
    }

###添加控制器
最后我们前去添加控制器。添加一个控制器类到`/Controllers`，命名为`ContactController `，并且确保使用了 __empty MVC controller__  视图。


    using MyFirstForm.Models;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using Umbraco.Web.Mvc;

    namespace MyFirstForm.Controllers
    {
        public class ContactFormController : SurfaceController
        {
            [HttpPost]
            public ActionResult Submit(ContactFormViewModel model)
            {
                if (!ModelState.IsValid)
                    return CurrentUmbracoPage();

                /// 在这里使用表单数据做些什么

                return RedirectToCurrentUmbracoPage();
            }
        }
    }

如果模型状态为无效，`CurrentUmbracoPage() `会使用户返回到表单。如果验证有效，你可以使用表单数据做些什么（例如发送邮件给网站管理员），并且会`RedirectToCurrentUmbracoPage();`

##将表单添加到模板
你可以通过输出局部视图把表单添加到模板中：

    @using MyFirstForm.Models;

    @{
        Html.RenderPartial("~/Views/Partials/ContactForm.cshtml", new ContactFormViewModel());
    }

##通过后台添加表单
把表单添加到网站我们可以使用宏。这也可以让编辑者使用富文本编辑器将表单添加到页面成为可能。

####创建宏
前往开发区块，右击 __Partial Views Macro Files__ 节点的菜单标签。将宏命名为 *Contact Form*，选择一个空的片段，并且使 __Create Macro__ 复选框选中。

在局部视图中，我们使用视图模型来输出之前创建的联系表单。

    @inherits Umbraco.Web.Macros.PartialViewMacroPage

    @using MyFirstForm.Models;

    @{
        Html.RenderPartial("~/Views/Partials/ContactForm.cshtml", new ContactFormViewModel());
    }


####添加宏
在能够添加表单到页面中，还要做最后一件事，就是允许在富文本中允许宏。选择并展开 __Macros__ 节点后选择 __Contact Form__ 宏。勾选 __Editor Settings__ 下面的复选框。

现在你就可以包含富文本编辑器的页面中插入表单了。

###更多信息
- [Surface Controllers](../../../Reference/Routing/surface-controllers.md)
- [Custom controllers](../../../Reference/Routing/custom-controllers.md)
- [Routing](../../../Reference/Routing/)



