## Jekyll Blog

- 新建博文：

  ```rake post title="Introduce of Qianmi" [category="category"] [date="2015-05-18"] [tags=[nodejs,javascript,...]] ```

  category默认blog，date默认当前时间，生成文件自动到分类目录下

- 新建文件

  ```rake page name="about.html"```

  name可以为子路径，类似"blog/about.html"
  name不包含文件扩展类型，默认在路径下生成"index.html"

- 本地运行

  ```rake preview```
  
## 带Disqus评论系统
