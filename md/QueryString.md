## 什么是查询字符串（URL参数）？

+ 查询字符串 Query String（URL参数）是指在URL的末尾加上用于向服务器发送信息的字符串（变量）。将“?”放在URL的末尾，然后再加上“参数＝值”，想加上多个参数的话，使用“&”。以这个形式，可以将想要发送给服务器的数据添加到URL中。

+ 例如，假设基本URL为“https：//○△×□.cn /”，则在基本URL中添加查询字符串（URL参数）为“https：//○△×□.cn /?●=▲×■＆○=△×□”。

+ 以上URL中“？●=▲×■＆○=△×□”的部分是查询字符串（URL参数）。


## Web服务上的用途和作用

+ 有两种查询字符串（URL参数），它们的用途不同。

### 网站访问分析（被动参数）

+ 被动参数**对显示的内容没有影响**。无论是否附加参数，显示的页面都是相同的。
  那么你为什么添加被动参数呢？那是为了进行Web网站的**访问分析**。为了了解用户从哪里到达了自己的网站，设定固有的参数来统计。
  通常，当您想要分析网站流入的来源以吸引客户，或者想要知道来自搜索引擎广告的流入客户时，可以使用它。

### 显示动态页面结果（活动参数）

+ 与被动参数不同，活动参数**会影响显示的内容**。换句话说，添加参数将改变网站上显示的内容。 
  例如，对于购物网站，您添加了活动参数，就可以按大小过滤产品或按价格排序。还可以在你选择一件S尺寸的T恤后，根据设定的尺寸过滤不相关的产品。 
  基本的T恤的商品一览页是「http : / /○○×□.cn / categris / tshirt /」。 
  具有S尺寸过滤的T恤的产品列表页面是“http : / /○○×□.cn / categris / tshirt /？T = shirt_size = S”。 
  但是，在上述情况下，您将能够有两个URL“http：//ju△×.jp.jp/category/tshirt/”。 相同的URL有两种可能会对SEO产生负面影响。 因此，我们建议使用专用的URL参数工具将URL设置为主页面。


## 添加查询字符串(url参数)时的注意点

+ 设定参数时，有几个注意点。为了向服务器传达正确的信息，有些事情是很重要的，请记住。

### 使用标准参数

+ 如果您使用非标准参数，例如使用“；”或“：”代替“=”或使用“[]”而不是“＆”，则搜索引擎无法正常读取。 因此，如上面的“http：//ju△×.jp.jp/category/tshirt/?T= shirt_size = S”所示，标准参数“？”和“=”，如果有多个参数让我们使用“＆”来设置它。

### ?只能用一次!

+ “?”仅用于参数的头部。 请注意，放置两个或多个网址无法正常使用。

### 页面内链接在最后面设置

+ 基本上，在URL的末尾加上参数。 但是，如果存在页内链接，则“URL +参数+页内链接”和页内链接将结束。 如果参数和页内链接相反，则会导致页内链接停止工作。


## 总结

+ 通过使用查询字符串（URL参数），可以区分用户的来源是否已知，以及它是来自自然搜索还是来自广告。 “URL参数”是提供用户足迹的参数。

+ 如果使用了url参数，就能比现在更有效率地进行web网站的集合。使用专用的“url参数工具”可以简单地设定。一定要试试。