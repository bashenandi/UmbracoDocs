# 登录界面

## 问候
登录界面具有问候功能，你可以个性化修改为你选择的语言。例如，对于 en-US，你可以添加下列 keys 到`~/Config/Lang/en-US.user.xml`文件：

    <area alias="login">
      <key alias="greeting0">Sunday greeting</key>
      <key alias="greeting1">Monday greeting</key>
      <key alias="greeting2">Tuesday greeting</key>
      <key alias="greeting3">Wednesday greeting</key>
      <key alias="greeting4">Thursday greeting</key>
      <key alias="greeting5">Friday greeting</key>
      <key alias="greeting6">Saturday greeting</key>
    </area>

你也可以在登录界面中设置其他自定义文本，从`~/Umbraco/Config/Lang/en.xml`中找到默认值，复制到你想要翻译到的`~/Config/Lang/MYLANGUAGE.user.xml`文件。

## 密码重置
"忘记密码？"连接允许你的后台用户重置他们的密码。想要使用这个功能，你需要在 web.config 文件中配置 SMTP 服务，并且指定"from"地址。例如：

    <system.net>
      <mailSettings>
        <smtp from="noreply@example.com">
          <network host="127.0.0.1" userName="username" password="password" />
        </smtp>
      </mailSettings>
    </system.net>

这个功能可以通过 `allowPasswordReset` 配置进行关闭, 参见: [安全](/Reference/Config/umbracoSettings/#security) 

## 背景图片
你可以自定义后台登录界面的背景图片。在 `~/Config/umbracoSetting.config` 中查找`loginBackgroundImage`，将其路径改为你想要使用的。

    <settings>
        <content>
            ...
            <loginBackgroundImage>/images/myCustomImage.jpg</loginBackgroundImage>        
        </content>
    </settings>
    
