# CSS 和 Javascript #

Looking at our homepage we’re obviously missing the CSS and JavaScript from the Initializr template. To include this navigate to the root of your website directory (e.g. "C:\inetpub\wwwroot" - this may be different depending on your installation type) in Windows Explorer and copy over the **_css\style.css_** file into the **_[your website root]\Css_** folder (replace Umbraco with wherever your Umbraco instance is being served from – e.g. “_C:\inetpub\wwwroot_”. Now refresh your webpage in your browser and you’ll see a more styled page.

我们的首页看起来很明显是丢了来自初始化模板的 CSS 和 JavaScript。想要包含这些文件，在资源管理器中导航到你的网站目录（例如 "C:\inetpub\wwwroot" - 根据你安装方式的不同，会有所不同）并且复制**_css\style.css_**到**_[your website root]\Css_**目录中（将目录替换为任何你的 Umbraco 实例所在的目录 - 例如: _C:\\inetpub\\wwwroot_ ）。现在刷新浏览器中的页面，你会看到更具样式的页面。


>注意 ：
>
> 你还可以使用 Umbraco 的 UI 来创建你的 CSS 文件。在**_Settings > Stylesheets_** > **_... > + Create_** 中创建一个样式表命名为 style(不要添加文件结尾.css)，然后粘贴前面我们复制的CSS到这个 CSS 里面。使用这两种方法都要注意，她们并**不会**自动被包含在 HTML 标记中 - Umbraco 的输出是简洁的，这意味着你只有在想要和需要它们的时候，手动引入它们。


接下来，要从初始化模板里的**js**目录中把**脚本**目录复制到 **_[your website root]\Scripts_**目录中 - 我们必须更改模板代码，用**/Scripts**来替换**/js**。前往**_Settings > Templates > Homepage_**，修改第21行的代码问为下面的内容，然后点击**_保存_**。

	<script src="scripts/libs/modernizr-2.0.6.min.js"></script>

>注意：
> 你还可以使用 Umbraco 的 UI 来创建你的 JavaScript 文件。在**_Settings > Scripts > ... > + Create_** (同样不要添加后缀，但是从**_Type_** 下拉列表中选择.js) 中创建脚本文件，在模板中使用“_scripts/myfile.js_”来进行引用。

现在使用Chrome打开开发者工具或者Firebug，同时浏览http://localhost你会看到 network 标签(或类似标签)，没有报告任何丢失素材/文件 - 如果有问题尝试修正任何拼写错误/检查文件在正确的位置。

## 下一步 - [输出文档类型属性](Outputting-the-Document-Type-Properties.md) ##
如何将 Umbraco 文档类型的属性包含在模板中，并将编写的数据输出到正确的位置。