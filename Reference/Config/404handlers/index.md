# 404handlers.config #

**过时的。*NotFoundHandlers*概念已经被Content Finders替代**

_配置文件用于旧的*NotFoundHandlers*。这用于在 Umbraco 内部管道中注册自定义代码，包含用于控制自定义404错误的旧的方法_

此配置文件中列出了“向后兼容”的默认处理程序：

    <NotFoundHandlers>
      <notFound assembly="umbraco" type="SearchForAlias" />
      <notFound assembly="umbraco" type="SearchForTemplate"/>
      <notFound assembly="umbraco" type="SearchForProfile"/>
      <notFound assembly="umbraco" type="handle404"/>
    </NotFoundHandlers>

## 向后兼容
如果你的 Umbraco 是从旧版本升级而来，旧的*NotFoundHandlers*依旧可以工作 - 请求管道包含了*ContentFinderByNotFoundHandlers*的IContentFinder，它会附加执行旧*NotFoundHandlers*功能，但是它会建议移动你的自定义请求控制器逻辑到IContentFinders。

## 显现404未找到属性

自定义404现在在自定义*IContentFinder*中处理，方法是将*IContentFinder*处理的*PublishedContentRequest*的IS404属性设置为true，或者将*IContentFinder*注册为*ContentLastChanceFinder*