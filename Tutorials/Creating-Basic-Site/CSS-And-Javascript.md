#CSS 和 Javascript
看起来我们的首页明显丢失了来自初始化模板的 CSS 和 JS。要包含这些，在 Windows 资源管理器中导航至你网站目录的根目录（例如 "C:\inetpub\wwwroot" - 这取决于你的安装方式而有所不同），复制**_css\style.css_**文件到**_[your website root]\Css_**目录中（不管里的Umbraco 实例安装在哪里用该目录替换这里 - 例如 "C:\inetpub\wwwroot"）。现在我们在浏览器中刷新网页，就会看到更加有风格页面。

>注意 – 你也可以使用 Umbraco 的 UI 来创建你的 CSS 文件。**_Settings > Stylesheets_** > **_... > + Create_** 会创建一个样式表称之为 style(不要添加文件结尾.css)，然后粘贴CSS 到里面。使用这两种方法都要注意，她们并**不会**自动被包含在 HTML 标记中 - Umbraco 的输出是简洁的，这意味着你只有在想要和需要它们的时候，手动引入它们。

下一步要复制**_脚本_**文件夹，从初始化模板里的**js**目录，把文件复制到 **_[your website root]\Scripts_**目录 - 我们不得不更改模板，用**/Scripts**替代**/js**。前往**_Settings > Templates > Homepage_**，修改第21行的代码问为下列的，然后点击**_保存_**。

	<script src="scripts/libs/modernizr-2.0.6.min.js"></script>

>注意 – 你还可以在后台 UI 中创建你的 JS 文件，前往**_Settings > Scripts > ... > + Create_** (同样不要添加后缀，但是从**_Type_** 下拉列表中选择.js) 创建，在模板中引入它们可以使用“_scripts/myfile.js_”。

现在使用Chrome打开开发者工具或者Firebug，同时浏览http://localhost你会看到 network 标签(或类似标签)，没有报告任何丢失素材/文件 - 如果有问题尝试修正任何拼写错误/检查文件在正确的位置。

---

##下一步 - [输出文档类型属性](Outputting-the-Document-Type-Properties.md)
如何关联文档类型属性到模板，用于输出编辑器内容到正确的地方。