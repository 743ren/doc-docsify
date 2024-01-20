基于 7.0

## 创建文档

创建文章 `hexo new post`，如果文件名有空格，用引号扩起来，`hexo new "post with whitespcae"`，真正创建的文章是 post-with-whitespace.md，里面的 title 是原来的 `post with whitespcae`。

`_config.yml` 中 `default_layout = post`，所以创建的文章默认在 source/_posts 目录下。如果指定为 page，就不在 _posts 目录下，而是在 source 目录下。

比如 `hexo new page test`，将创建 `source/test/index.md`，也可以指定路径 `hexo new page --path test/me me`，这样将创建 `source/test/me.md`

layout 除了 post、page 还有 draft，如 `hexo new draft test`，将生成 `source/_drafts/test.md``

## 命令

- hexo clean 清除缓存和 public 文件
- hexo s(server) 启动本地预览服务
- hexo g(generate) 在 public 目录生成静态文件

上面三个命令都可以添加选项 --draft 表示包含草稿 _draft 目录里的文章
`hexo publish [layout] <title>`，将 _draft 里的文章移动到别的发布目录。

## 目录和标签

```yaml
categories:
  - Diary
  - Life
```

会使分类 Life 成为 Diary 的子分类，而不是并列分类。

```yaml
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
```

此时这篇文章同时包括三个分类：PlayStation 和 Games 分别都是父分类 Diary 的子分类，同时 Life 是一个没有子分类的分类。

```yaml
tags:
- PS3
- Games
```

tags 不像 categories，没有顺序层级关系。

## 引用块

```
{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}
```

- author 是作者
- source 是来源，比如书名，表示 content 是这个人在哪里说的，如果有它，要用逗号隔开
- link 是超链接
- source_link_title 如果配了这个，不会显示完整的超链接，而是显示这个文字，点击后也是跳转到这个地址

比如：

{% blockquote 李白, 静夜思 https://baike.baidu.com/item/静夜思/214 百科-静夜思 %}
床前明月光
{% endblockquote %}


{% blockquote @央视新闻 https://weibo.com/u/2656274875 %}
央视新闻
{% endblockquote %}

## 资源

在 source 目录建立文件夹，和 _post 同级，比如一张图片路径是 /source/assets/2023/img.jpg，在文章中路径就是 /assets/2023/img.jpg。

同时 `_config.yml` 的 post_asset_folder 保存 false，`_posts` 文件夹只会渲染 md 文件，如果改成了 true，在这里的文件也会被复制到最终的静态文件中，但建立 post 类型的 md 文件，就自动建一个同名的文件夹，太多了，不好。

## 部署

如果是部署到 Github Pages，在 `_config.yml` 中配置

```yaml
deploy:
  type: 'git'
  repo: git@github.com:xxx/xxx.git
  branch: main
```

还有先执行 `npm install hexo-deployer-git --save`，然后 `hexo d(deploy) -g` 先生成再自动部署，是把 public 文件夹发布到仓库。

部署到 Vercel，不需要上面配置，直接把源码传到库，日志没有任何错误，但不知怎么打开就是空白。于是到 Netlify 部署，报错了，说 theme 没有加 submodule 配置，索性直接把 theme 下的 .git 删了，然后部署成功，再回到 Vercel，发现它也能打开，大概也是这问题。

## Butterfly 主题

基于 4.11

安装 `npm install hexo-theme-butterfly`
升级 `npm update hexo-theme-butterfly`
应用 `theme: butterfly`

将主题目录的 `_config.yml` 复制到 Hexo 根目录，重命名为 `_config.butterfly.yml`，将覆盖主题目录的配置，和项目根目录配置合并，且优先级更高。

### 导航栏

```yaml
nav:
  logo: # 最上面导航栏logo图片
  display_title: true # 是否显示文字标题
  fixed: false # 是否固定在页面最上面
```

### 右上方的菜单

```yaml
menu:
  首页: / || fas fa-home # 名称: 路径 || 图标名称
  归档: /archives/ || fas fa-archive
  标签: /tags/ || fas fa-tags
  分类: /categories/ || fas fa-folder-open
  清单||fas fa-list: # 展开的列表
    音乐: /music/ || fas fa-music
    照片: /Gallery/ || fas fa-images
    视频: /movies/ || fas fa-video
  友链: /link/ || fas fa-link
  关于: /about/ || fas fa-heart
```

### 头像

```yaml
avatar:
  img: https://i.loli.net/2021/02/24/5O1day2nriDzjSu.png
  effect: false # 是否自动旋转
```

### 社交账号图标

```yaml
social:
  fab fa-github: https://github.com/xxx || Github || '#24292e'
  fas fa-envelope: mailto:xxx@gmail.com || Email || '#4a7dbe'
  fa-brands fa-weibo: https://weibo.com/u/xxx || 微博 || '#24292e'
  fa-brands fa-x-twitter: https://twitter.com/xxx || Twitter || '#24292e'
  fa-brands fa-discord: https:xxx || Discord || '#24292e'
  fa-brands fa-telegram: https:aaa || Telegram || '#24292e'
  fa-brands fa-weixin: https:bbb || 微信 || '#24292e'
  fa-brands fa-qq: https:ccc || QQ || '#24292e'
  fa-brands fa-zhihu: https:ddd || 知乎 || '#24292e'
  fa-brands fa-bilibili: https:eee || 哔哩哔哩 || '#24292e'
  fa-brands fa-tiktok: https:fff || 抖音 || '#24292e'
  fa-brands fa-youtube: https:ggg || 油管 || '#24292e'
```

图标是 [fontawesome](https://fontawesome.com/icons)，后面跟着链接，鼠标移上去显示的名称和图标颜色。

一些没有的图标比如快手、小红书、豆瓣之类，[到iconfont找](https://butterfly.js.org/posts/4073eda/#Icon)

### 分类和标签顶部图

```yaml
tag_img: 不配 tag_per_img 时的默认图片
tag_per_img:
  标签1: 图片地址1
  标签2: 图片地址2
category_img: 不配 category_per_img 时的默认图片
category_per_img:
  分类1: 图片地址1
  分类2: 图片地址2
```

类似 /source/categories/index.md 这种 page 页面图片在 md 中的 front-matter 中的 top_img 配置。

### 文章图

上面是page的顶部图，post文章的顶部图也是配置 top_img，值可以有如下情况：
- 可以图片链接
- 可以是 transparent 表示透明
- 可以是 false 表示不显示
- 可以是颜色值，如 rgb(0,0,255)、orange、'linear-gradient( 135deg, #E2B0FF 10%, #9F44D3 100%)'(要单引号)

文章显示在首页的封面是 cover，如果没有配置，将用主题配置里的 default_cover，可以配置多个路径，值和 top_img 一样，不想显示的话也可以设置成 false

文章顶部图，先取 top_img，取不到再取 cover，还取不到，取主题配置里的 default_top_img，所以如果不需要区分 top_img 和 cover 时，只配置 cover 就行了。

### 版权

主题设置 post_copyright 的 enable 为 true，每篇文章下都有版权信息。如果是转载，不需要版权，在文章内 front_matter 加 copyright: false。

可以在文章内单独修改默认的版权信息

```yaml
copyright_author: xxxx
copyright_author_href: https://xxxxxx.com
copyright_url: https://xxxxxx.com
copyright_info: 此文章版权归xxxxx所有，如有转载，请注明来自原作者
```

### 打赏

主题配置文件

```yaml
reward:
  enable: true
  text:
  QR_code:
    - img: /img/wechat.jpg
      link:
      text: 微信
    - img: /img/alipay.jpg
      link:
      text: 支付宝
```

配置后每篇文章的结尾会有一个“打赏”按钮。如果没有二维码，可配置一张软件的 icon 图片，然后在 link 上添加相应的打赏链接。点击图片就会跳转到链接去，如果是二维码，不需要跳转链接，link 可以不写。

### 页脚

footer:
  owner:
    enable: true
    since: 2022 # 博客从什么年份开始
  custom_text: Hi, welcome to my <a href="https://网址/">blog</a>! # 可以用来显示 ICP 备案号
  copyright: true

### 侧边栏

```yaml
aside:
  card_author: # 作者信息
    enable: true
    description: # 账号名下面的描述文字，不设置会取 Hexo 配置文件的 description
    # button: 下面一个关注按钮
    #   enable: true
    #   icon: fab fa-github
    #   text: Follow Me
    #   link: https://github.com/xxxxxx
    card_announcement: # 一个小喇叭公告，如果有社群，可以放一些社区信息，图片
      enable: false
      content: This is my Blog

newest_comments:
  enable: true # 侧边栏显示最新评论
```

[侧边栏自定义 Widget](https://butterfly.js.org/posts/ea33ab97/)

### 数学公式

```yaml
katex:
  enable: true
  per_page: false
  hide_scrollbar: true
```

按照网站配置设置，per_page 为 false，就是不会每页自动渲染，如果这个页面有数学公式，在文章的 front_matter 里加 katex: true。

### 文章内图库

```yaml
<div class="gallery-group-main">
{% galleryGroup name description link img-url %}
{% galleryGroup name description link img-url %}
</div>
```

- name：图库名字
- description：图库描述
- link：连接到对应相册的地址，一个 page 页面
- img-url：图库封面的地址

比如创建两个页面，用来显示旅游图片 `hexo new page --path travel/beijing beijing`、`hexo new page --path travel/shanghai shanghai`。然后写

```
<div class="gallery-group-main">
{% galleryGroup '北京' '北京拍的图片' '/travel/beijing' https://images.pexels.com/photos/19872/pexels-photo.jpg %}
{% galleryGroup '上海' '上海拍的图片' '/travel/shanghai' https://images.pexels.com/photos/745243/pexels-photo-745243.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2 %}
</div>
```

然后点击封面跳转的页面，travel/beijing.md 里的图片格式写成

```yaml
{% gallery [lazyload],[rowHeight],[limit] %}
markdown 图片格式
{% endgallery %}
```

- lazyload 点击按钮加载更多图片，默认为 false。
- rowHeight 图片显示的高度，如果需要一行显示更多的图片，可设置更小的数字。默认为 220。
- limit 每次加载多少张照片，默认为 10。

```yaml
{% gallery true,100,15 %}
![](https://i0.hippopx.com/photos/477/768/839/swimmer-sport-swim-water-preview.jpg)
![](https://i0.hippopx.com/photos/298/434/513/beach-dawn-dusk-ocean-preview.jpg)
![](https://i0.hippopx.com/photos/242/966/413/dawn-sun-mountain-landscape-preview.jpg)
![](https://i0.hippopx.com/photos/653/876/844/road-forest-season-autumn-preview.jpg)
![](https://i0.hippopx.com/photos/299/326/436/write-plan-business-startup-preview.jpg)
![](https://i0.hippopx.com/photos/170/829/152/summerfield-woman-girl-sunset-preview.jpg)
![](https://i0.hippopx.com/photos/179/171/625/sparkler-holding-hands-firework-preview.jpg)
![](https://i0.hippopx.com/photos/146/950/505/legs-window-car-dirt-road-preview.jpg)
{% endgallery %}
```

### 系列文章

文章 front-matter 写 series: xxx，然后在别的文章如果自己也写了相同的 series: xxx，那么文章中写 `{% series %}`，如果自己的 front-matter 没写 series: xxx，那么文章中写 `{% series xxx %}`，就会将有 series: xxx 的文章用列表显示出来，样式比较简陋。

### 其它

mermaid 不是 markdown 那语法，得[单独配置](https://butterfly.js.org/posts/4aa8abbe/#mermaid)

[友情链接](https://butterfly.js.org/posts/dc584b87/#友情鏈接)现在也不需要。

可以配置[在线聊天](https://butterfly.js.org/posts/ceeb73f/#在綫聊天)，网站有客服的时候可以配。

个人博客用不着[分析统计](https://butterfly.js.org/posts/ceeb73f/#分析統計)。

没什么访客的情况下，也用不着放[广告](https://butterfly.js.org/posts/ceeb73f/#廣告)

[全局吸底音乐播放具体介绍](https://butterfly.js.org/posts/507c070f/)，测试 QQ 音乐不行，不能自动播放，data-autoplay 为 true 是每次点击空白都自动切换到下一首了。

侧边栏显示的运行时间

```yaml
runtimeshow:
  enable: true
  publish_date: 2020/1/1 00:00:00 # 调整为开通时间或最早一篇文章时间
```

### 问题

- 评论系统用的 Twikoo，不能配置，只能全局，page 页面也有评论。
- 不好修改 favicon。

## 插件

- [abbrlink](https://github.com/rozbo/hexo-abbrlink) 文章链接变成一个字符串，有利于 SEO。`hexo clean` 之后才能看到效果。类似的插件 [hexo-friendly-link](https://github.com/houyonglu/hexo-friendly-link)。
- [hexo-filter-nofollow](https://github.com/hexojs/hexo-filter-nofollow) 为网站使用到的所有外链添加 `rel="noopener external nofollow noreferrer"`，可以有效地加强网站 SEO 和防止权重流失
- [hexo-bridge](https://github.com/DeepSpaceHarbor/hexo-bridge) 在线管理博客，通过 http://localhost:4000/bridge/ 访问，显示所有 posts/page/主题/插件
- [hexo-generator-alias](https://github.com/hexojs/hexo-generator-alias) 文章里链接失效可以配置别名，相当于重定向，在 Hexo 配置中加
  ```yml
  alias:
    api/index.html: api/classes/Hexo.html
    plugins/index.html: https://github.com/tommy351/hexo/wiki/Plugins
    ```
- [hexo-hide-posts](https://github.com/prinsss/hexo-hide-posts) 将文章隐藏，但可以通过链接访问，可以控制在哪些地方隐藏，哪些地方显示，文章的 front-matter 配置 hidden: true 来隐藏
- [hexo-reference](https://github.com/kchen0x/hexo-reference) 支持脚注语法
- [hexo-tag-plantuml](https://github.com/two/hexo-tag-plantuml) 支持渲染 plantuml
- [hexo-tag-mmedia](https://github.com/u2sb/hexo-tag-mmedia) 在文档中显示各种音频视频
- [hexo-tag-map](https://github.com/kuole-o/hexo-tag-map) 在文档中显示地图
- [hexo-pdf](https://github.com/superalsrk/hexo-pdf/) 在文档中显示pdf
- ~~[hexo-douban-card](https://github.com/TankNee/hexo-douban-card) 文章中显示豆瓣信息的卡片。比如 `{% douban book 30376420 %}`，会在根目录下生成一个 json 文件，json 里面看图片链接能显示出图片，但是它的卡片上就显示不出来。~~
- [hexo-lazyload-image](https://github.com/Troy-Yang/hexo-lazyload-image) 图片延迟加载，滑到那再加载
- [hexo-readmore](https://github.com/rqh656418510/hexo-readmore)、[hexo-plugin-readmore](https://github.com/snowdreams1006/hexo-plugin-readmore) 公众号引流，想看完整的文章引导关注公众号
- [hexo-minify](https://github.com/lete114/hexo-minify) 资源压缩

## 搜索引擎收录

用插件 [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap) 或者 [hexo-generator-seo-friendly-sitemap](https://github.com/ludoviclefevre/hexo-generator-seo-friendly-sitemap) 生成 sitemap.xml。

到 [Google](https://search.google.com/search-console/welcome) 验证所有权，然后在“站点地图”里提交 https://域名/sitemap.xml 地址即可。

到 [Bing](https://www.bing.com/webmasters/about) 直接从 Google 导入网站配置。

[百度站长平台](https://ziyuan.baidu.com/site/index#/)，不想被它搜索，没设置。

## 多语言

language 配置支持的语言。

```yaml
language:
  - zh-CN
  - en
```

修改配置 `new_post_name: :lang/:title.md`，创建文件的命令改成 `hexo new <title> --lang en`，就是将参数的 lang 变成文件夹，这样如果不加参数的话，就自动将文件放到 _post/undefined 文件夹下，当然也可以不加这配置，直接手动创建放到对应文件夹下，比如 _post/en、_post/ja。

`permalink: :lang/:year-:month-:day/:title/` 将 :lang 放到最后的 link 的最前面，这样生成的文件 url 地址就是 `http://localhost:4000/en/2022-12-13/test/` 这种，加上配置 `i18n_dir: :lang` 后，会自动检测路径 path 的第一个部分，比如检测到 en，那么主题文件就会去找 en 的配置。

但是它并不是那种多语言切换版本，一篇文章点击另一种语言就能切换到翻译版本，而是一篇文档打开整个主题的语言变换。

安装插件 `npm install hexo-generator-i18n --save`

默认配置就是 

```yaml
i18n:
  type:
    - post
    - page
  generator:
    - index
    - archive
    - category # 表示分类页也生成对应翻译版本
    - tag
```

安装插件 `npm install hexo-generator-index-i18n --save` 

hexo-generator-index-i18n 的 single_language_index 为 false，主页上显示所有语言的文章，如果为 true，将为每种语言生成一个首页，找到 index_generator，添加 single_language_index。

```yaml
index_generator:
  single_language_index: true
```

这样的话，访问 http://localhost:4000 相当于 http://localhost:4000/zh-CN/，只显示文章 front-matter 里 lang: zh-CN 的文章。而 http://localhost:4000/en/ 显示的就只有 lang: en 的文章。

并且如果有同名文件，`permalink: :lang/:year-:month-:day/:title/` year/month/day 也一样，那么就可以当成多种语言的翻译版本。比如 http://localhost:4000/zh-CN/1999-09-09/test 对应的翻译版本就是 http://localhost:4000/en/1999-09-09/test。

使用 abbrlink 插件，设置 `permalink: :lang/:abbrlink/` 也一样，同名文件，只是语言不同，生成的链接 id 是一样的。

配合主题添加顶部切换按钮，在主题配置文件的 menu 下添加一个选项

```yaml
menu:
  归档: /archives/ || fas fa-archive
  语言||fas fa-language:
    🇨🇳 简体中文: /zh-CN/
    🇺🇸 English: /en/
```

不过归档/标签之类的地方看到的还是中文，譬如“归档”，因为配的是 /archives/，而不是 /en/archives/，虽然可以通过类似 `hexo generate --config custom.yml,custom2.json,custom3.yml` 来指定不同配置，相同值后面的覆盖前面的，但是我这是部署到 Vercel 上的，且也不好在切换语言时才变。

