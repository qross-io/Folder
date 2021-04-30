# Voyager 缓存

因为模板引擎要从 HTML 或 Markdown 文件中读取内容再解析后呈现给客户端，缓存可以在将文件被访问一次后保存到缓存中，下次读取时直接读取缓存中的内容。建议在生产环境启用缓存，而在开发环境中关闭缓存。

启停用缓存使用全局设置`voyager.cache.enabled=true`，设置方式请参照 [Voyager 设置](/voyager/setup.md)。

---
参考链接

* [Voyager 设置](/voyager/setup.md)
* [Markdown 文件支持](/voyager/markdown.md)