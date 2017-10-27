---
title: Hexo之NexT主题配置
date: 2017-6-28
tags: [hext, next]
categories: HEXO
author: AdamZeng
copyright: true
top: 1
---
# **<center>hexo之next主题个性化配置详细教程</center>** #



## **1.在右上角或者左上角实现fork me on github** ##


**实现的效果图**

>![](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhifork-me-on-github.png)

**具体实现方法**
<!-- more -->
>点击[传送门>>](https://github.com/blog/273-github-ribbons)
>挑选自己喜欢的样式，并复制代码。 例如，我是复制如下代码：![](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhifork-me-on-github-demo.png)

>然后粘贴刚才复制的代码到
>themes/next/layout/_layout.swig
>文件中
```html
<div class="headband"></div>
```
的下面，并把href改为你的github地址!
```html
<a href="https://github.com/wongzeqi">
```

![](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhifork-me-on-github-code.png)



## **2.添加RSS** ##
**实现效果图**

>![](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhiRSS.png)

**具体实现方法**
>切换到hexo初始化的目录上然后安装 Hexo 插件：(这个插件会放在node_modules这个文件夹里)


``` bash
	$ npm install --save hexo-generator-feedalert(s);
```
>接下来打开站点配置文件 _config.yml 在里面的末尾添加(请注意在冒号后面要加一个空格，不然会发生错误！)



	   # Extensions 
	   ## Plugins: http://hexo.io/plugins/
	   plugins: hexo-generate-feed
 
 
>然后打开next主题文件夹里面的_config.yml,在里面配置为如下样子：(就是在rss:的后面加上/atom.xml,注意在冒号后面要加一个空格)

	 # Set rss to false to disable feed link.
	 # Leave rss as empty to use site's feed link.
	 # Set rss to specific value if you have burned your feed already.
	 rss: /atom.xml
>配置完之后运行：
```bash 
$ hexo g
```
>重新生成一次，你会在 ./public 文件夹中看到 atom.xml 文件。然后启动服务器查看是否有效，之后再部署到 Github 中。
## **3. 添加动态粒子背景** 
 **实现效果图**
 
 ![背景图片效果](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhibg.png)
 
 **具体实现方法**
 
 >修改_layout.swig
 打开 next/layout/_layout.swig
 在 < /body>之前添加代码(注意不要放在< /head>的后面)
 
    {% if theme.canvas_nest %}
	<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js">
	</script>
	{% endif %}

>修改配置文件
>打开 /next/_config.yml在里面添加如下代码：(可以放在最后面)

	# --------------------------------------------------------------
	# background settings
	# --------------------------------------------------------------
	# add canvas-nest effect
	# see detail from https://github.com/hustcc/canvas-nest.js
	canvas_nest: true
>到此就结束了，运行 hexo clean，然后运行 hexo g,然后运行 hexo s，最后打开浏览器在浏览器的地址栏输入 localhost:4000 就能看到效果了\（￣︶￣）/

>**如果你感觉默认的线条太多的话**
可以这么设置====>
在上一步修改 _layout.swig中，把刚才的这些代码：

	{% if theme.canvas_nest %}
	<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
	{% endif %}
>**说明**
配置项说明

	color ：线条颜色, 默认: '0,0,0'；三个数字分别为(R,G,B)
	opacity: 线条透明度（0~1）, 默认: 0.5
	count: 线条的总数量, 默认: 150
	zIndex: 背景的z-index属性，css属性用于控制所在层的位置, 默认: -1
>实现点击出现桃心效果

在网址输入如下
	 http://7u2ss1.com1.z0.glb.clouddn.com/love.js
	然后将里面的代码copy一下，新建love.js文件并且将代码复制进去，然后保存。
	将love.js文件放到路径/themes/next/source/js/src里面，
	然后打开\themes\next\layout\_layout.swig文件,在末尾（在前面引用会出现找不到的bug）
	添加以下代码：
```javascript
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/love.js"></script>
```

## **4. 修改文章内链接文本样式**
>**实现效果图**

![效果图](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhilink.gif)


>**具体实现方法**

修改文件 themes\next\source\css\_common\components\post\post.styl ，在末尾添加如下css样式，：
```css
// 文章内链接文本样式
.post-body p a{
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```
其中选择 .post-body 是为了不影响标题，选择 p 是为了不影响首页“阅读全文”的显示样式,颜色可以自己定义。


## **5. 修改文章底部的那个带#号的标签**
>**实现效果图**

![tags](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhitargs.png)

>**具体实现方法**

修改模板
/themes/next/layout/_macro/post.swig，搜索 rel="tag">#，将 # 换成
```html 
<i class="fa fa-tag"></i>
```

## **6. 在每篇文章末尾统一添加“本文结束”标记**
>**实现效果图**

![end](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/peizhiend.png)

具体实现方法

在路径 \themes\next\layout\_macro 中新建 passage-end-tag.swig 文件,并添加以下内容：

```html
<div>
	{% if not is_index %}
		<div style="text-align:center;color: #ccc;font-size:14px;">
			-------------本文结束
			<i class="fa fa-paw"></i>
			感谢您的阅读-------------
		</div>
	{% endif %}
</div>
```
	
	
接着打开\themes\next\layout\_macro\post.swig文件，在post-body 之后， post-footer之前添加如下画红色部分代码（post-footer之前两个DIV）：如下大概在360行左右的位置：

![code](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/end-code.png)

代码如下：
```html
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```
然后打开主题配置文件_config.yml,在末尾添加：

	# 文章末尾添加“本文结束”标记
	passage_end_tag:
	  enabled: true
## **7. 修改作者头像并旋转**

>**实现效果图**--------------------------------------------------无~0.0~

>**具体实现方法**

打开\themes\next\source\css\_common\components\sidebar\sidebar-author.styl，在里面添加如下代码：
```css
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;

  /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;

  /* 设置循环动画 [animation: (play)动画名称 (2s)动画播放时长单位秒或微秒 (ase-out)动画播放的速度曲线为以低速结束 
    (1s)等待1秒然后开始动画 (1)动画播放次数(infinite为循环播放) ]*/
 

  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;
}

img:hover {
  /* 鼠标经过停止头像旋转 
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/

  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}

/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}
```

## **8. 主页文章添加阴影效果**

>**实现效果图**

![效果图](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/as.png)


>**具体实现方法**

打开\themes\next\source\css\_custom\custom.styl,向里面加入：

```css
/*主页文章添加阴影效果*/
 .post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```
## **9. 在网站底部加上访问量**
>**实现效果图**

![效果图](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/number.png)

>**具体实现方法**

打开\themes\next\layout_partials\footer.swig文件,在copyright前加上画红线这话：

![code](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/number-code.png)

代码如下：
```javascript
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
然后再合适的位置添加显示统计的代码(位置还是上述这个文件)，如图：

![code](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/number-code-2.png)

代码如下：
```html
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>
```

## **10. 网站底部字数统计和时间统计**
>**实现效果图**

![效果图](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/hanzi.png)
>**具体实现方法**

文字统计切换到根目录下，然后运行如下代码
```bash
$ npm install hexo-wordcount --save
```
然后在/themes/next/layout/_partials/footer.swig文件尾部加上：
```html
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>
```

时间统计在根目录下安装 hexo-wordcount,运行：
```bash
$ npm install hexo-wordcount --save
```
然后在主题的配置文件中，配置如下：

	# Post wordcount display settings
	# Dependencies: https://github.com/willin/hexo-wordcount
	post_wordcount:
    item_text: true
	wordcount: true
	min2read: true





## **11. 在文章底部增加版权信息**
>**实现效果图**

![这里写图片描述](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/power.png)


>**请看具体实现步骤**-------------------------**哈哈哈哈哈**

在目录next/layout/_macro/下添加my-copyright.swig：
```html
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>
  
  <!-- JS库 sweetalert 可修改路径 -->
  <script type="text/javascript"  src="http://jslibs.wuxubj.cn/sweetalert_mini/jquery-1.7.1.min.js"></script>
  <script src="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.min.js"></script>
  <link rel="stylesheet" type="text/css" href="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.mini.css">
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
    clipboard.on('success', $(function(){
      $(".fa-clipboard").click(function(){
        swal({   
          title: "",   
          text: '复制成功',   
          html: false,
          timer: 500,   
          showConfirmButton: false
        });
      });
    }));  
</script>
{% endif %}
```
在目录next/source/css/_common/components/post/下添加my-post-copyright.styl：
```css
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```
修改next/layout/_macro/post.swig，在代码
```html
<div>
      {% if not is_index %}
        {% include 'wechat-subscriber.swig' %}
      {% endif %}
</div>
```
之前添加增加如下代码：
```html
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```
如图：
![这里写图片描述](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/copyright.png)



修改next/source/css/_common/components/post/post.styl文件，在最后一行增加代码：

@import "my-post-copyright"
保存重新生成即可。如果要在该博文下面增加版权信息的显示，需要在 Markdown 中增加copyright: true的设置，类似：

	---
	title: 前端小项目：使用canvas绘画哆啦A梦
	date: 2017-05-22 22:53:53
	tags: canvas
	categories: 前端
	copyright: true
	---

## **12. 文章加密访问**

>**直接看步骤**

打开themes->next->layout->_partials->head.swig文件,在以下位置插入这样一段代码：

![code](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/password.png)

```javascript
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```

最后在你的文章中添加password标记如图：
![效果图](http://os94ofsac.bkt.clouddn.com/hexo-next-blog/password-xiaoguo.png)


## **<center>今天就写到这里          未完待续......</center>**


