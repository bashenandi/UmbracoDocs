# 订阅事件

订阅事件允许你在一批事件发生前和后执行你自己定义的代码。你需要准备一些 Umbraco 安装好的内容，跟着下面的指导操作。例如Fanoe 初学者工具包。

### 订阅到一个事件
让我们在文档发布时，添加一段文本字符串到日志中。

首先我们要添加一个新的类到项目中。称之为 *LogWhenPublished*。事件在核心服务中，所以添加引用`using Umbraco.Core;`，然后我们需要继承application事件处理器`: ApplicationEventHandler`

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Web;
	using Umbraco.Core;
	
	public class LogWhenPublished : ApplicationEventHandler{
		///Here we'll subscribe to an event
	}

第一件要做的事是在*LogWhenPublished*中重写`ApplicationStarted`，然后订阅事件到到`Umbraco.Core.Services.Contentservice.Published`。

	protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext){
		Umbraco.Core.Services.ContentService.Published += ContentService_Published;
	}

执行你的自定义代码还需要添加（如果你使用 VS，你可以通过在代码段中的`+=`后面连按两次tab 键键很容易的添加下面的代码）：

	void ContentService_Published(Umbraco.Core.Publishing.IPublishingStrategy sender, Umbraco.Core.Events.PublishEventArgs<Umbraco.Core.Models.IContent> e){
		// Your custom code goes here
	}

现在来检查一下，当事件发生时，它会添加一段消息到日志中。我们使用 LogHelper 来做这些，所以添加 `using Umbraco.Core.Logging;` 然后添加：

	LogHelper.Info(typeof(LogWhenPublished), "Something has been published");

整个类看起来如下：

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Umbraco.Core;
    using Umbraco.Core.Logging;

    public class LogWhenPublished : ApplicationEventHandler
    {
        protected override void ApplicationStarted(UmbracoApplicationBase umbracoApplication, ApplicationContext applicationContext)
        {
            Umbraco.Core.Services.ContentService.Published += ContentService_Published;
        }

        void ContentService_Published(Umbraco.Core.Publishing.IPublishingStrategy sender, Umbraco.Core.Events.PublishEventArgs<Umbraco.Core.Models.IContent> e)
        {
            // LogHelper will write to tracelog.txt
            LogHelper.Info(typeof(LogWhenPublished), "Something has been published");
        }
    }

前往后台发布一些内容。现在打开`App-Data/Logs/`目录中的tracelog.txt，滚动到文档底部，可以找到输入的日志。

![跟踪日志中的信息](images/log-message.png)

### 之前和之后
你可以看到当我们发布了一些内容时我们的自定义代码会执行。事实上执行是在发布之后是因为我们使用的是`Published`事件。如果我们想要在发布前执行代码，使用`Publishing `。其他类似的方法还有：`Saving` ， `Saved`， `Copying` ， `Copied`等等。

### 更多信息
- [事件参考](../../../Reference/Events/)