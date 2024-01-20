[官方文档](https://docsify.js.org/#/zh-cn/)

本地预览执行 `docsify serve`

1. 在标题后写 `<!-- {docsify-ignore} -->` 排除在侧边栏显示，在文档第一个标题后添加 `<!-- {docsify-ignore-all} -->` 可以忽略文档标题。
2. 文件名后加内容以更好的支持 SEO，比如 `[Guide](guide.md "The greatest guide in the world")`。
3. [扩展语法](https://docsify.js.org/#/zh-cn/helpers)，包含强调内容，图片尺寸，html 和markdown 混用等。
4. [直接显示被嵌入的文件内容](https://docsify.js.org/#/zh-cn/embed-files)。
5. 搜索配置有一项叫 `pathNamespaces: ['/en']`，就是如果有多语言，当切换到 en 时，只在 en 目录搜索，不然会搜到中文里的内容。
6. 用了 PWA 模式，导致本地预览无法自动刷新，清除浏览器数据才行，还是别开启了。
7. 评论系统 Disqus 需要科学上网。waline 或 valine 用的 leancloud，国内要备案，国际版真正发评论时也得科学上网。Gitalk 要填一些id/secret，且库必须公开，文件一看源代码全都显示出来。cusdis 如果自托管需要连接一个服务器上的数据库。算了。
8. CDN 配的这个 `<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>` 不能用，会引起搜索框出错，页面无法滑动。
9. 代码高亮要支持一种语言就要加一句 script，比如要支持 java，就要添加 `<script src="//cdn.jsdelivr.net/npm/prismjs@1/components/prism-java.min.js"></script>`，支持的语言种类在 [Prism (prismjs.com)](https://prismjs.com/#supported-languages)。
10. `formatUpdated: '{YY}-{MM}-{DD} {HH}:{mm}'`，记录的是文章最后的修改时间，然后在文档里面插入`{ docsify-updated }`(*需要把大括号左右两边空格去掉，这里为了显示加上，因为一去掉就解析成时间值了*)，会解析成定义格式的日期时间。[docsify-updated](https://github.com/pfeak/docsify-updated) 插件自动在每个页面显示更新时间，代替 `formatUpdated` 配置。但是部署到 Vercel 后，两种方式都无法获取到时间。
11. mermaid 组件的支持
	文档有两句被注释掉了，因为它的说明文档不需要引入，但实际上是需要的，css 放到 head 里，script 放到 body 里。然后文档上的
	
	```javascript
	var num = 0; 
	mermaid.initialize({ startOnLoad: false });
	```

	这两句也极其的莫名其妙，不知道放在那里，后来用[Docsify画图建模Mermaid插件支持](https://blog.csdn.net/jslygwx/article/details/125868321)写的方法可以正常显示，和官方的代码对比，发现官方的配置只要用使用下面的就能正常显示：
	```javascript
	markdown: {
		renderer: {
			code: function(code, lang) {
				if (lang === "mermaid") {
					// 最大的问题就是原来 code 这里写的是 
					// mermaid.render('mermaid-svg-' + num++, code)
					// 但这句执行有问题，不懂
					return ('<div class="mermaid">' + code + "</div>");
				}
				return this.origin.code.apply(this, arguments);
			}
		}
	}
	```
12. Footer 加点东西
	```javascript
	plugins: [
		function(hook) {
			var footer = [ // 就是字符串拼接出 html 语句
				'<hr/>',
				'<footer>',
				'<span>&copy;2023.</span>',
				'<span>Powered by ',
				'<a href="https://docsify.js.org/#/" target="_blank">docsify</a> v',
				window.Docsify.version, // 能获取到当前用的版本
				'</span>',
				'</footer>'
				].join('');
			hook.afterEach(function(html) {
				return html + footer;
			});
		}
	]
	```
13. [更多的插件](https://docsify.js.org/#/zh-cn/awesome?id=plugins)。
    - [docsify-puml](https://github.com/indieatom/docsify-puml)：支持 plantuml 解析显示。
	- [docsify-corner](https://github.com/Koooooo-7/docsify-corner)：自定义右上角折角。
	- [docsify-sidebar-collapse](https://github.com/iPeng6/docsify-sidebar-collapse)：在侧边栏 `_sidebar` 里写的多级目录可以折叠展开。
	- [docsify-kroki](https://github.com/zuisong/docsify-kroki)：支持各种图形渲染，包括 mermaid，plantuml，没有试用。
	- [docsify-drawio](https://github.com/KonghaYao/docsify-drawio)：支持 drawio 绘图。
	- [docsify-image-caption](https://h-hg.github.io/docsify-image-caption/)：优化图片显示效果，标题在最下方，可以显示在文字行内。
	- [docsify-versioned-plugin](https://github.com/UliGall/docsify-versioned-plugin)：文档的多版本选择。
	- [docsify-termynal](https://github.com/sxin0/docsify-termynal)：一个终端窗口。
	- [docsify-hide-code](https://github.com/jl15988/docsify-hide-code)：代码块不全部展示，折叠起来。
	- [docsify-busuanzi](https://github.com/mg0324/docsify-busuanzi)：不蒜子访问统计。
	- ~~[docsify-slides](https://github.com/shawntabrizi/docsify-slides)：在文档中写 `<!-- slide:break -->` 将文档变成两列布局显示。~~影响封面显示。
	- ~~[docsify-dark-mode](https://github.com/Plugin-contrib/docsify-plugin/tree/master/packages/docsify-dark-mode)：深色模式切换。~~主题色配置不太好调，影响所有的文字颜色。