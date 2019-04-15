# FindPublishedContentAndTemplate() #

这个方法在"PublishedContentRequest.PrepareRequest()"方法中调用。我们将简要讨论此方法的使用：

1. FindPublishedContent ()
2. Handles redirects
3. HandlePublishedContent()
4. FindTemplate()
5. FollowExternalRedirect()
6. HandleWildcardDomains()

#### HandlePublishedContent ####
- 没有内容?
 - 运行LastChanceFinder
 - 是一个 IContentFinder, 通过ContentLastChanceFinderResolver解析
 - 默认，为空 (= ugly 404)
- 遵循内部重定向
 - 注意无限循环
- 确保用户有权访问发布的内容
 - 否则重定向到登录页面或者拒绝访问已经发布的内容
- 没有内容时循环
 - 注意无限循环

#### FindTemplate ####
- Use altTemplate if
 - Initial content
 - Internal redirect content, and InternalRedirectPreservesTemplate is true
- No alternate template?
 - Use the current template if one has already been selected
 - Else use the template specified for the content, if any
- Alternate template?
  - Use the alternate template, if any
  - Else use what’s already there: a template, else none
- Alternate template is used only if displaying the intended content
  - Except for internal redirects
  - If you enable InternalRedirectPreservesTemplate
  - Which is false by default
- Alternate template replaces whatever template the finder might have set
  - ContentFinderByNiceUrlAndTemplate
  - /path/to/page/template1?altTemplate=template2  template2
- Alternate template does not falls back to the specified template for the content
  - /path/to/page?altTemplate=missing  no template
  - Even if the page has a template
- But preserves whatever template the finder might have set
  - /path/to/page/template1?altTemplate=missing  template1
- Yes, these rules are arbitrary… feedback?  Get to the [Dev Group](https://groups.google.com/forum/#!forum/umbraco-dev)!

#### FollowExternalRedirect() ####
- content.GetPropertyValue<string>("umbracoRedirect")
- 如果存在，则将已发布的内容请求设置为重定向到内容
- 将触发外部（浏览器）重定向


#### HandleWildcardDomains() ####

![](images/culture-and-hostnames.png)

- 查找之间最深的通配符域
 - 根域 (或者顶级)
 - 请求的已发布内容
- 如果找到，相应地更新请求的区域性

这实际上实现了主机名和语言之间的分离。
