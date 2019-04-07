---
title: hexo NexT主题配置与美化
tags:
  - hexo
  - next
  - 教程
categories: 教程
image: 'http://wx4.sinaimg.cn/mw690/00639ahCgy1g1txkcpk68j30hs07sjrm.jpg'
abbrlink: a9d4a14e
date: 2019-04-07 12:42:11
permalink:
copyright:
description: Hexo 是高效的静态站点生成框架，她基于 Node.js。 通过 Hexo 你可以轻松地使用 Markdown 编写文章，除了 Markdown 本身的语法之外，还可以使用 Hexo 提供的 标签插件 来快速的插入特定形式的内容。在这篇文章中，假定你已经成功安装了 Hexo，并使用 Hexo 提供的命令创建了一个站点。
---
<p class="description"></p>

<!-- more -->

## 1. 页面底部添加网站运行时间

1. 打开`themes/next/_config.yml`，添加

   ```
    # -------------------------------------------------------------
    # web run time
    # 这是网站开始运行的时间，展示网站运行了多长时间。
    # 格式, 2018-02-13 15:00:00. 如果个数只有一位，需要补零。
    runtime:
   enable: true
   year: 2018
   month: 02
   day: 13
   hour: 15
   minute: 00
   second: 00
   ```

2. 在`themes\next6\layout\`下新建`web-runtime.swig`，写入如下内容，保存。

   ```
    <div>
      <span id="sitetime"></span>
      // 这里我没有好办法吧swig的值传到JavaScript代码中。如果您会，请帮我改进。
      <span id="year" style="display:none">{{theme.footer.runtime.year}}</span> 
      <span id="month" style="display:none">{{theme.footer.runtime.month}}</span>   
      <span id="day" style="display:none">{{theme.footer.runtime.day}}</span>   
      <span id="hour" style="display:none">{{theme.footer.runtime.hour}}</span>   
      <span id="minute" style="display:none">{{theme.footer.runtime.minute}}</span>   
      <span id="second" style="display:none">{{theme.footer.runtime.second}}</span>   
   	<script language=javascript>
   	function siteTime(){        
   		window.setTimeout("siteTime()", 1000);
   		var seconds = 1000;
   		var minutes = seconds * 60;
   		var hours = minutes * 60;
   		var days = hours * 24;
   		var years = days * 365;
   		var today = new Date();
   		var todayYear = today.getFullYear();
   		var todayMonth = today.getMonth()+1;
   		var todayDate = today.getDate();
   		var todayHour = today.getHours();
   		var todayMinute = today.getMinutes();
   		var todaySecond = today.getSeconds();
   		/* Date.UTC() -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
   		year - 作为date对象的年份，为4位年份值
   		month - 0-11之间的整数，做为date对象的月份
   		day - 1-31之间的整数，做为date对象的天数
   		hours - 0(午夜24点)-23之间的整数，做为date对象的小时数
   		minutes - 0-59之间的整数，做为date对象的分钟数
   		seconds - 0-59之间的整数，做为date对象的秒数
   		microseconds - 0-999之间的整数，做为date对象的毫秒数 */        
   		var year = document.getElementById("year").innerHTML;
   		var month = document.getElementById("month").innerHTML;
   		var day = document.getElementById("day").innerHTML;
   		var hour = document.getElementById("hour").innerHTML;
   		var minute = document.getElementById("minute").innerHTML;
   		var second = document.getElementById("second").innerHTML;//北京时间2018-2-13 00:00:00
   		var t1 = Date.UTC(year,month,day,hour,minute,second); 
   		var t2 = Date.UTC(todayYear,todayMonth,todayDate,todayHour,todayMinute,todaySecond);
   		var diff = t2-t1;
   		var diffYears = Math.floor(diff/years);
   		var diffDays = Math.floor((diff/days)-diffYears*365);
   		var diffHours = Math.floor((diff-(diffYears*365+diffDays)*days)/hours);
   		var diffMinutes = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours)/minutes);
   		var diffSeconds = Math.floor((diff-(diffYears*365+diffDays)*days-diffHours*hours-diffMinutes*minutes)/seconds);
   		if(diffYears==0){
   		document.getElementById("sitetime").innerHTML=" Website has run  "/*+diffYears+" year "*/+diffDays+" days "+diffHours+" h "+diffMinutes+" min "+diffSeconds+" s";
   		} else{
   		document.getElementById("sitetime").innerHTML=" Website has run  "+diffYears+" year "+diffDays+" days "+diffHours+" h "+diffMinutes+" min "+diffSeconds+" s";
   		}
   	}
   	//siteTime(document.getElementById("year").innerHTML,document.getElementById("year").innerHTML,document.getElementById("year").innerHTML,document.getElementById("year").innerHTML,document.getElementById("year").innerHTML,0);
   	siteTime();
   	</script>
   </div>
   ```

3. 打开`themes\next6\_layout\footer.swig`，在您想展示的位置，比如备案代码之前，加入如下内容。

   ```
   {% if theme.footer.runtime.enable %}
     {% include 'web-runtime.swig' %}  
   {% endif %}
   ```

4. 结束，刷新网页查看效果。

## 2. 文章结束处添加感谢阅读的提

1. 打开`themes/next/_config.yml`，添加

   ```
    # At the end of the article, show thanks for reading
   end_info:
     enable: true
     start_info: -------The end of this article
     icon: paw
     end_info: Thank you for your reading-------
   ```

2. 在`themes\next6\_macro\`下新建`passage-end-tag.swig`，写入如下内容，保存。

   ```
   <div>
      {% if not is_index %}
   		<div style="text-align:center;color: #ccc;font-size:14px;">{{theme.end_info.start_info}}&nbsp;<i class="fa fa-{{ theme.end_info.icon }}"></i>&nbsp;{{theme.end_info.end_info}}</div>
   	{% endif %}
   </div>
   ```

3. 打开`themes\next6\_macro\post.swig`，在如下代码(相关文章)之前，

   ```
   {% if theme.related_posts.enable and (theme.related_posts.display_in_home or not is_index) %}
        {% include 'post-related.swig' with { post: post } %}
      {% endif %}
   ```

   加入

   ```
   <div>
        {% if not is_index and theme.end_info.enable %}
          {% include 'passage-end-tag.swig' %}
        {% endif %}
      </div>
   ```

4. 结束，刷新网页查看效果

## 3. 文章中添加对应的多语言版本的链接

这个功能需要手动设置对应文章的`abbrlink`一致。建议在本地先编译一个语言版本的文件，然后手动修改另一个版本的文章的链接。

1. 打开`themes/next/_config.yml`，添加

   ```
   # Go to another language Page
      translation:
         enable: true
         language: English  # language name
         icon: globe
         info: 英文版本 
         url: https://en.xian6ge.cn # Destination URL
   ```

2. 在`themes\next6\_partials\`下新建`post-tran.swig`，写入如下内容，保存。

   ```
   <div>
      <ul class="post-copyright">
   <li class="post-copyright-link">
   	<strong><i class="fa fa-{{ theme.translation.icon }}"></i> {{ theme.translation.language + __('symbol.colon') }}</strong>
   	<span id="url" style="display:none">{{theme.translation.url}}</span> 
   	<span id="path" style="display:none">{{post.permalink}}</span> 
   	<span id="info" style="display:none">{{theme.translation.info}}</span> 
   	<span id="goto"></span> 
   </li>
   </ul>
   
   //我不知道怎么整理链接，所以只能用JavaScript重写。
   <script language=javascript>
   	var url = document.getElementById("url").innerHTML;
   	var path = document.getElementById("path").innerHTML;
   	var info = document.getElementById("info").innerHTML;
   	path=GetUrlRelativePath(path);
   	if(info.length==0){
   	infos= url+path;
   	}
   	else{
   	infos=info;
   	}
   	var str = "<a href='"+url+path+"'>"+infos+"</a>"
   	console.log(str);
   	document.getElementById("goto").innerHTML=str;
   
   　　function GetUrlRelativePath()
   　　{
   　　　　var url = document.location.toString();
   　　　　var arrUrl = url.split("//");
   
   　　　　var start = arrUrl[1].indexOf("/");
   　　　　var relUrl = arrUrl[1].substring(start);//stop省略，截取从start开始到结尾的所有字符
   
   　　　　if(relUrl.indexOf("?") != -1){
   　　　　　　relUrl = relUrl.split("?")[0];
   　　　　}
   　　　　return relUrl;
   　　}
   </script>
   ```

3. 打开`themes\next6\_macro\post.swig`，在POST BODY之前，加入

   ```
   {% if theme.translation.enable and not is_index %}
     <div>
          {% include '../_partials/post-tran.swig' with { post: post } %}
          <br/>
     </div>
   {% endif %}
   ```

4. 结束，刷新网页查看效果



## 4. 解决左边工具栏上无法跳转到外部链接的问题

由于NexT6做了设置，左边工具栏上的所有链接将会自动在前面添加上当前域名。对于，“首页”，“关于”这样的链接没有问题。但如果要添加上外链，例如英文页面：`https:\\en.myliwu.work`，会被自动编译为`https:\\en.myliwu.work\https\en.myliwu.work`，造成跳转出错。为此，我们可以采用一个折中的办法。

在`hexo\source`下建立一个`en.html`文件，在里面通过JavaScript代码跳转。但这样会遇到一个问题，Hexo会编译所有文件，造成其中的Js代码失效。因此，在前面添加`layout: false`设置，告诉编译器不要编译该HTML文件。具体代码如下。

```
layout: false
title: "XianliuGe - Eternal charm, Endless sound."
date: 2018-11-13 09:12:12
---
<!--上述title采用对应网站的title，以减少跳转时的突兀感。-->
<!DOCTYPE html>
<html lang="en">

	<head>

		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<title>XianliuGe - Eternal charm, Endless sound.</title>
        <script type="text/javascript">
            window.location.href = "https://en.myliwu.work";
        </script>
		<!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
		<!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
		<!--[if lt IE 9]>
            <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
            <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script>
        <![endif]-->

		<!--[if gte IE 9]>
        	<style type="text/css">
        		.gradient {
        			filter: none;
        		}
        	</style>
        <![endif]-->

		<!-- Favicon and touch icons -->
		<!-- <link rel="shortcut icon" href="assets/ico/favicon.png">
		<link rel="apple-touch-icon-precomposed" sizes="144x144" href="assets/ico/apple-touch-icon-144-precomposed.png">
		<link rel="apple-touch-icon-precomposed" sizes="114x114" href="assets/ico/apple-touch-icon-114-precomposed.png">
		<link rel="apple-touch-icon-precomposed" sizes="72x72" href="assets/ico/apple-touch-icon-72-precomposed.png">
		<link rel="apple-touch-icon-precomposed" href="assets/ico/apple-touch-icon-57-precomposed.png"> -->

	</head>

	<body>

		<!-- Loader -->
		<h1>前往英文版</h1>
		<h2>Go to XianliuGe for English</h2>		

	</body>

</html>
```



## 5. 更新提示

更新会有不确定因素，

#### 其它更新

- npm 更新全局安装的包：

```
npm update -g
```

- npm 更新站点文件夹根目录下安装的依赖包：

```
所在目录：~/blog/npm update
```

- 更新 npm 它自己：

```
npm install npm -g
```

- 更新 Node.js 到最新版：

```
npm install n -g

n latest
```

#### 更新主题

进入主题文件夹根目录，然后`git pull`，发现报错，怎么解决呢？可以先浏览[这篇文章](http://www.01happy.com/git-resolve-conflicts/)，然后参考我的操作。

先到主题文件夹根目录：

```
所在目录：~/blog/themes/next/git pull
```

会发现报错，由于我们更改了相关文件，更新不成功，所以要将本地的所有修改先暂时存储起来：

```
所在目录：~/blog/themes/next/git stash
```

然后再试一下：

```
所在目录：~/blog/themes/next/git pull
```

可以了吧，接下来还原暂时存储的内容（即保存我们的所有修改）：

```
所在目录：~/blog/themes/next/git stash pop
```

如果报`CONFLICT`，是因为 Git 无法确定一些改动，所以要我们手动解决文件中冲突的部分，这个比较麻烦，可以参考我下面的流程。

首先打开报`CONFLICT`的文件，Ctrl + F 搜索`>>>>>>> Stashed changes`，查看从此处到`=======`之间保存的代码，回忆一下自己当时更改了什么，是为了达到什么功能。

然后查看`=======`到`<<<<<<< Updated upstream`之间更新的代码，与下面保存的代码进行对比（也请浏览下所标出代码前后的代码）：

1. 如果改动较大，可能是主题增加了新功能，建议保留更新的代码，然后更改一下，达到自己想要在保存的代码中实现的功能，最后删除保存的代码。
2. 如果改动较小，建议还是保留更新的代码，然后更改一下，最后删除保存的代码。

> 要是自己不确定，一定记得将`<<<<<<< Updated upstream`到`>>>>>>> Stashed changes`之间的代码另存备份，然后进行调试，直到最后确定没有问题。

最后：

```
所在目录：~/blog/themes/next/root@kali:~/blog/themes/next# git pull
error: Pulling is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.
```

又报错了

先查看：

```
所在目录：~/blog/themes/next/root@kali:~/blog/themes/next# git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   layout/_custom/header.swig
	modified:   layout/_custom/sidebar.swig
	modified:   layout/_layout.swig
	modified:   layout/category.swig
	modified:   layout/tag.swig
	modified:   source/css/_common/components/post/post-meta.styl
	modified:   source/css/_common/components/post/post-nav.styl
	modified:   source/css/_common/scaffolding/base.styl
	modified:   source/css/_custom/custom.styl
	deleted:    source/images/avatar.gif
	modified:   source/lib/Han/dist/han.min.css

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	both modified:   _config.yml
	both modified:   languages/zh-Hans.yml
	both modified:   layout/_macro/post-copyright.swig
	both modified:   layout/_macro/post.swig
	both modified:   layout/_macro/sidebar.swig
	both modified:   layout/_partials/footer.swig
	both modified:   layout/page.swig
	both modified:   source/css/_variables/base.styl

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	layout/_macro/passage-end-tag.swig
	source/js/src/love.js
```

看下面`Unmerged paths`，说`git reset HEAD <file>...`来取消修改（大概），`git add <file>...`来 mark 决定（大概），我们当然要保存这些文件的更改，所以：

```
所在目录：~/blog/themes/next/root@kali:~/blog/themes/next# git add _config.yml languages/zh-Hans.yml layout/_macro/post-copyright.swig layout/_macro/post.swig layout/_macro/sidebar.swig layout/_partials/footer.swig layout/page.swig source/css/_variables/base.styl layout/_macro/passage-end-tag.swig source/js/src/love.js
```

顺便把新加的`passage-end-tag.swig`和`love.js`也加进去，最后再来试一下吧：

```
所在目录：~/blog/themes/next/root@kali:~/blog/themes/next# git pull
Already up to date.
```

哇，成功更新主题！

> 注意：更新有风险，一定要谨慎处理文件中冲突的部分！

> 另外：如果更新 NexT 主题后，配置文件有些新功能不会配置，可以查看 [Releases](https://github.com/iissnan/hexo-theme-next/releases) 页面，去里面找说明。



## 6. 站点配置文件

请先查看 [Hexo 官方文档](https://hexo.io/zh-cn/docs/configuration.html)，再查看下面我贴出的，如果这样后你还是对有些地方比较懵，可以自行 Google。

如果你的文件中没有相关内容，请勿直接添加，且所有的`:`都为英文字符，它后面都有一个空格。

```
[2017.11.14 更新] 文件位置：~/blog/_config.yml# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: muzhi
subtitle: 
description: 
author: muzhi
language: zh-CN
timezone:

# URL
url: https://myliwu.work/
root: /
# 博客文章的 URL 结构，请务必写文章之前就想好！
# 详细参数请查看：https://hexo.io/docs/permalinks.html
permalink: :category/:year/:month/:day/:title.html
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
# 代码高亮设置
highlight:
  enable: true
  line_number: true
# 代码自动高亮
  auto_detect: true
  tab_replace:

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
# URL 中的分类和标签「翻译」成英文
# 见：https://github.com/hexojs/hexo/issues/1162
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

## 修改归档页面、某一分类页面、某一标签页面的显示篇数
## 参考：http://theme-next.iissnan.com/faqs.html#setting-page-size
archive_generator:
  per_page: 0
  yearly: false
  monthly: false
  daily: false

category_generator:
  per_page: 0

tag_generator:
  per_page: 0

# Extensions
## Plugins: https://hexo.io/plugins/
# RSS，要先进入站点文件夹根目录安装插件
# npm install hexo-generator-feed --save 即可
# 无需更多配置
# 参数说明查看 README：https://github.com/hexojs/hexo-generator-feed
feed:
  type: atom
  path: atom.xml
# 文章数，0 为全部
  limit: 0
  hub:
# 是否包含文章内容
  content: true

githubEmojis:
  enable: true
  idName: github-emoji
  unicode: false
  styles:
  localEmojis:

## Themes: https://hexo.io/themes/
# 主题配置
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/reuixiy/reuixiy.github.io.git
  branch: master
```

## 7. 主题配置文件

如果你的主题不是 NexT，那么请另 Google。建议先查看 [NexT 官方文档](http://theme-next.iissnan.com/getting-started.html)，写的很好，多逗留会没坏处。

```
# ---------------------------------------------------------------
# Theme Core Configuration Settings
# ---------------------------------------------------------------

# 更新相关，参考：
# https://github.com/iissnan/hexo-theme-next/issues/328
override: false

# ---------------------------------------------------------------
# Site Information Settings
# ---------------------------------------------------------------

# 站点图标，直接去 https://realfavicongenerator.net
# 选项弄好后，下载压缩包，解压复制粘贴
# 建议放在 hexo-site/source/images/ 里
# 这样可以避免更新 NexT 主题的时候遇到麻烦
# 最后记得要稍微改下文件名，与下面的保持一致
favicon:
  small: /images/favicon-16x16.png
  medium: /images/favicon-32x32.png
  apple_touch_icon: /images/apple-touch-icon.png
  safari_pinned_tab: /images/safari-pinned-tab.svg
  android_manifest: /images/manifest.json
  ms_browserconfig: /images/browserconfig.xml

# Set default keywords (Use a comma to separate)
# 站点关键字，利于 SEO 大概，记得用英文逗号分隔
keywords: reuixiy,哲学,Hexo,博客,Coldplay

# Set rss to false to disable feed link.
# Leave rss as empty to use site's feed link.
# Set rss to specific value if you have burned your feed already.
# 则无需在这添加任何东西
rss:

footer:
# 页脚配置
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  # 建站年份
  since: 2017

  # Icon between year and copyright info.
  # 年份后面的图标，为 Font Awesome 图标
  # 自己去纠结 https://fontawesome.com/v4.7.0/
  # 然后更改名字就行，下面的有关图标的设置都一样
  icon: heart

  # If not defined, will be used `author` from Hexo main config.
  # 如果不定义，默认用站点配置文件的 author 名
  copyright:
  # -------------------------------------------------------------
  # Hexo link (Powered by Hexo).
  # Hexo 的链接
  powered: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    version: false
  # -------------------------------------------------------------
  # Any custom text can be defined here.
  # 自定义内容，要加记得把前面的 # 去掉
  #custom_text:

# ---------------------------------------------------------------
# SEO Settings
# ---------------------------------------------------------------

canonical: true

seo: false

# If true, will add site-subtitle to index page, added in main hexo config.
# subtitle: Subtitle
index_with_subtitle: false


# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# 菜单设置 || 菜单图标设置（图标上面说了，不重复）
# 项目换行可以更改显示顺序
# 如果这个项前会显示 .menu
# 解决方法：编辑 ~/blog/themes/next/languages 下的相应文件
# 比如添加一个「留言」菜单，站点配置文件的 language 是 zh-CN
# 则编辑 zh-CN.yml，在 menu: 项内添加一行 留言: 留言
# 注意空格，且符号 : 为英文字符！
menu:
  home: / || home
  archives: /archives/ || archive
  categories: /categories/ || th
  tags: /tags/ || tags
  top: /top/ || signal
  about: /about/ || futbol-o
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat

# Enable/Disable menu icons.
# 是否开启菜单图标
menu_icons:
  enable: true

# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
# 设计板式，都长啥样，去 README 里面的链接里看看
# https://github.com/iissnan/hexo-theme-next#live-preview
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini


# ---------------------------------------------------------------
# Sidebar Settings
# ---------------------------------------------------------------

# 侧栏社交链接设置，与上面菜单差不多，要生效记得把前面的 # 去掉
#social:
  #GitHub: https://github.com/yourname || github
  #E-Mail: mailto:yourname@gmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

# 侧栏社交链接图标设置
social_icons:
  enable: true
  icons_only: false
  transition: false

# Blog rolls
# 侧栏友链设置
links_icon: globe
links_title: 神奇的链接
#links_layout: block
links_layout: inline
links:
  网易云音乐: https://music.163.com/#/user/home?id=86590096
  Coldplay Official Website: http://coldplay.com/
  获取 Elon Musk 的新闻: https://elonmusknews.org/
  尼古拉·特斯拉：发明了现代世界的人: http://www.bilibili.com/video/av6211226/
  关于此博客: https://reuixiy.github.io/about/

# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
# 侧栏头像设置
# 建议放在 hexo-site/source/uploads/ 里（没有自己建）
# 这样可以避免更新 NexT 主题的时候遇到麻烦
avatar: /uploads/avatar.gif

# Table Of Contents in the Sidebar
# 侧栏文章目录设置（前提是 Markdown 书写正确）
toc:
  enable: true

  # Automatically add list number to toc.
  # 自动加数字序号
  number: true

  # 如果标题太长，则放到下一行继续显示
  wrap: true

# Creative Commons 4.0 International License.
# http://creativecommons.org/
# Available: by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero
#creative_commons: by-nc-sa
#creative_commons:

sidebar:
  # Sidebar Position, available value: left | right (only for Pisces | Gemini).
  # 侧栏位置设置，可用值：左 | 右（只对 Pisces 和 Gemini 设计版式有效！）
  position: left
  #position: right

  # Sidebar Display, available value (only for Muse | Mist):
  # 侧栏显示方式，post 代表只有点进一篇文章内
  # 且文章有目录，侧栏才会弹出显示
  #display: post
  #display: always
  display: hide
  #display: remove

  # Sidebar offset from top menubar in pixels (only for Pisces | Gemini).
  # 只对 Pisces 和 Gemini 设计版式有效！
  offset: 12

  # Back to top in sidebar (only for Pisces | Gemini).
  # 只对 Pisces 和 Gemini 设计版式有效！
  b2t: false

  # Scroll percent label in b2t button.
  # 在回到顶部按钮里显示阅读百分比
  scrollpercent: true

  # Enable sidebar on narrow view (only for Muse | Mist).
  # 移动端显示侧栏，只对 Muse 和 Mist 设计版式有效！
  onmobile: true


# ---------------------------------------------------------------
# Post Settings
# ---------------------------------------------------------------

# Automatically scroll page to section which is under <!-- more --> mark.
# 点击 [Read More]，页面自动滚动到 <!-- more --> 标记处
scroll_to_more: false

# Automatically saving scroll position on each post/page in cookies.
# 用 cookies 保存浏览的位置信息，意味着重新打开这个页面后
# 页面就会自动滚动到上次的位置，除非读者清理浏览器 cookies
save_scroll: false

# Automatically excerpt description in homepage as preamble text.
# 将每篇文章 Front-matter 里 description 的文字作为页面显示的文章摘要
excerpt_description: false

# Automatically Excerpt. Not recommend.
# Please use <!-- more --> in the post to control excerpt accurately.
# 按字数自动加入 [Read More]，不建议！
# 建议在文章中加入 <!-- more -->
# 自定义 [Read More] 按钮之前要显示的内容！
auto_excerpt:
  enable: false
  length: 150

# Post meta display settings
# 文章顶部显示的文章元数据设置
post_meta:
  item_text: true
  created_at: true
  updated_at: false
  categories: true

# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
# 显示统计字数和估计阅读时长
# 注意：这个要安装插件，先进入站点文件夹根目录
# 然后：npm install hexo-wordcount --save
post_wordcount:
  item_text: true
  wordcount: true
  min2read: false
  totalcount: false
  separated_meta: false

# Wechat Subscriber

# Reward

# Declare license on posts
post_copyright:
  enable: false
  license: CC BY-NC-SA 3.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/3.0/


# ---------------------------------------------------------------
# Misc Theme Settings
# ---------------------------------------------------------------

# Reduce padding / margin indents on devices with narrow width.
# 移动端把页面两边留白去除，个人不建议
# 移动端样式可以参考本文 4.2 节的相关代码
mobile_layout_economy: false

# Android Chrome header panel color ($black-deep).
# Android 上 Chrome 浏览器顶部颜色设置
android_chrome_color: "#fff"

# Custom Logo.
# !!Only available for Default Scheme currently.
# Options:
#   enabled: [true/false] - Replace with specific image
#   image: url-of-image   - Images's url
custom_logo:
  enabled: false
  image:

# Code Highlight theme
# Available value:
#    normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme
# 代码高亮主题设置
# 都长啥样自己点开上面的链接查看
highlight_theme: normal
# 注意要先在站点配置文件中设置


# ---------------------------------------------------------------
# Font Settings
# - Find fonts on Google Fonts (https://www.google.com/fonts)
# - All fonts set here will have the following styles:
#     light, light italic, normal, normal italic, bold, bold italic
# - Be aware that setting too much fonts will cause site running slowly
# - Introduce in 5.0.1
# ---------------------------------------------------------------
font:
  enable: true

  # Uri of fonts host. E.g. //fonts.googleapis.com (Default)
  host: https://fonts.loli.net

  # Font options:
  # `external: true` will load this font family from `host` above.
  # `family: Times New Roman`. Without any quotes.
  # `size: xx`. Use `px` as unit.

  # Global font settings used on <body> element.
  global:
    external: true
    family: Lato
    size:

  # Font settings for Headlines (h1, h2, h3, h4, h5, h6).
  # Fallback to `global` font settings.
  headings:
    external: true
    family: Roboto Slab
    size:

  # Font settings for posts.
  # Fallback to `global` font settings.
  posts:
    external: true
    family:

  # Font settings for Logo.
  # Fallback to `global` font settings.
  logo:
    external: true
    family:
    size:

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family: Roboto Mono
    size:


# ---------------------------------------------------------------
# Third Party Services Settings
# ---------------------------------------------------------------

# MathJax Support
mathjax:
  enable: false
  per_page: false
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML

# Han Support docs: https://hanzi.pro/
# 汉字标准格式，没用过暂时不了解
han: false

# Swiftype Search API Key

# Baidu Analytics ID

# Duoshuo ShortName

# Disqus

# Hypercomments
# 我用的就是这种，特点是可以匿名评论，暂时还没有加入 GFW 名单
# https://www.hypercomments.com/
# 自己去网站用 Google 邮箱注册，有啥不懂请留言评论
# 建议你把 Quote（管理台右上角的 Settings 按钮，然后 Widget 里）关了
# 鼠标在一些段落上时，这个 Quote 会在后面显示一个图标
# 或者选中一段文字，会有……
hypercomments_id:

# changyan

# Valine.

# Support for youyan comments system.

# Support for LiveRe comments system.
# 来必力评论
# 参见教程：https://linan.blog/2017/LiveReCommentsSystem/
#livere_uid: your uid

# Gitment

# Baidu Share

# Share

# Google Webmaster tools verification setting
# See: https://www.google.com/webmasters/
# Google 站长工具校验
# 可以不用这种验证方式，直接先添加下面的 Google Analytics
# 然后用 Google Analytics 校验
#google_site_verification:

# Webmaster 是用来提交自己的文章链接给 Google
# 然后别人就有可能通过 Google 搜索到自己的博客
# 这个必须做，如果你的文章想被别人看到的话


# Google Analytics
# 去 https://analytics.google.com 注册，自备梯子
google_analytics:

# Bing Webmaster tools verification setting

# Yandex Webmaster tools verification setting

# CNZZ count

# Application Insights

# Make duoshuo show UA

# Post widgets & FB/VK comments settings.
# ---------------------------------------------------------------
# Facebook SDK Support.

# Facebook comments plugin

# VKontakte API Support.

# Star rating support to each article.
# To get your ID visit https://widgetpack.com
rating:
  enable: true
  id:
  color:  f79533
# ---------------------------------------------------------------

# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
# 可以显示每篇文章的阅读量
# 然后可以通过阅读量建立 TopX 页面，教程链接：
# https://notes.wanghao.work/2015-10-21-为NexT主题添加文章阅读量统计功能.html
leancloud_visitors:
  enable: true
  app_id:
  app_key:

# Another tool to show number of visitors to each article.

# Show PV/UV of the website/page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi/
# 不蒜子统计，用于在页脚显示总访客数和总浏览量，将 false 改为 true 就能直接使用
busuanzi_count:
  # count values only if the other configs are false
  enable: true
  # custom uv span for the whole site
  site_uv: true
  site_uv_header: <i class="fa fa-user-circle-o"></i>
  site_uv_footer:
  # custom pv span for the whole site
  site_pv: true
  site_pv_header:
  site_pv_footer: <i class="fa fa-eye"></i>
  # custom pv span for one page only
  # 页面浏览量，不建议开启，建议用上面的 leancloud_visitors
  # 首先 leancloud 更稳定，其次 leancloud 便于管理
  # 最后，可以利用 leancloud 的 api 建立 TopX 页面
  page_pv: false
  page_pv_header: <i class="fa fa-file-o"></i>
  page_pv_footer:

# Tencent analytics ID

# Tencent MTA ID

# Enable baidu push
baidu_push: false

# Google Calendar

# Algolia Search

# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
# 要安装插件才能使用，先进入站点文件夹根目录
# 然后：npm install hexo-generator-searchdb --save
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1


# ---------------------------------------------------------------
# Tags Settings
# ---------------------------------------------------------------

# External URL with BASE64 encrypt & decrypt.
# Usage: {% exturl text url "title" %}
# Alias: {% extlink text url "title" %}
# 用法见：
# https://github.com/iissnan/hexo-theme-next/pull/1438
exturl: false

# Note tag (bs-callout).
# 主题的标签样式，有 note、label、tabs 三种
note:
  style: flat
  icons: true
  border_radius: 3
  light_bg_offset: 0

# Label tag.
label: true

# Tabs tag.
tabs:
  enable: true
  transition:
    tabs: true
    labels: true
  border_radius: 0

#! ---------------------------------------------------------------
#! DO NOT EDIT THE FOLLOWING SETTINGS
#! UNLESS YOU KNOW WHAT YOU ARE DOING
#! ---------------------------------------------------------------

# Use velocity to animate everything.
motion:
  enable: true
  async: true
  transition:
    post_block: perspectiveLeftIn
    post_header: fadeIn
    post_body: fadeIn
    coll_header: perspectiveLeftIn
    # Only for Pisces | Gemini.
    sidebar: slideUpIn

# Fancybox
# 查看图片的
fancybox: true


# Progress bar in the top during page loading.
# 页面顶部加载条
pace: true
pace_theme: pace-theme-flash

# Canvas-nest
canvas_nest: false

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false

# Only fit scheme Pisces
# Canvas-ribbon
# size: The width of the ribbon.
# alpha: The transparency of the ribbon.
# zIndex: The display level of the ribbon.
canvas_ribbon:
  enable: false
  size: 300
  alpha: 0.6
  zIndex: -1

# Script Vendors.
# Set a CDN address for the vendor you want to customize.
# 相关内容用 CDN 地址取代，加速网站访问，注意版本尽可能要一致
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib

  # Internal version: 2.1.3
  jquery: https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js

  # Internal version: 2.1.5
  # See: http://fancyapps.com/fancybox/
  # 自定义 fancybox（暂时）
  fancybox: https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.2.5/jquery.fancybox.min.js
  fancybox_css: https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.2.5/jquery.fancybox.min.css

  # Internal version: 1.0.6
  # See: https://github.com/ftlabs/fastclick
  fastclick: https://cdnjs.cloudflare.com/ajax/libs/fastclick/1.0.6/fastclick.min.js

  # Internal version: 1.9.7
  # See: https://github.com/tuupola/jquery_lazyload
  lazyload: https://cdnjs.cloudflare.com/ajax/libs/jquery_lazyload/1.9.7/jquery.lazyload.min.js

  # Internal version: 1.2.1
  # See: http://VelocityJS.org
  velocity: https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.1/velocity.min.js

  # Internal version: 1.2.1
  # See: http://VelocityJS.org
  velocity_ui: https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.1/velocity.ui.min.js

  # Internal version: 0.7.9
  # See: https://faisalman.github.io/ua-parser-js/
  ua_parser: https://cdnjs.cloudflare.com/ajax/libs/UAParser.js/0.7.9/ua-parser.min.js

  # Internal version: 4.6.2
  # See: http://fontawesome.io/
  fontawesome: https://cdn.bootcss.com/font-awesome/4.7.0/css/font-awesome.css

  # Internal version: 1.0.2
  # See: https://github.com/HubSpot/pace
  # Or use direct links below:
  pace: //cdn.bootcss.com/pace/1.0.2/pace.min.js
  pace_css: //cdn.bootcss.com/pace/1.0.2/themes/blue/pace-theme-flash.min.css

# Assets
css: css
js: js
images: images

# Theme version
version: 5.1.3

# 文章末尾添加「本文结束」标记，请勿直接添加，教程链接：
# http://shenzekun.cn/hexo的next主题个性化配置教程.html
passage_end_tag:
  enabled: true
```



## 8. 附上 custom.styl

一定是先 F12 找到要自定义的元素，调试成自己喜欢的值，然后再复制到`custom.styl`，而不是直接复制我给出的，我给出的仅供参考。

请先找对元素，不然可能会制造出新 bug，建议大家修改一个，就加个注释，方便以后调试修改。

```
[2017.12.31 更新] 文件位置：~/blog/themes/next/source/css/_custom/custom.styl// Custom styles
// 页面最顶部的横线
.headband {
    height: 1.5px;
    background-image: linear-gradient(90deg, #F79533 0%, #F37055 15%, #EF4E7B 30%, #A166AB 44%, #5073B8 58%, #1098AD 72%, #07B39B 86%, #6DBA82 100%);
}
// 页面顶部行高
.header {
    line-height: 1.5;
}
// 页面背景色
.container {
    background-color: rgba(0, 0, 0, 0.75);
}
// 页面留白更改
.header-inner {
    padding-top: 0px;
    padding-bottom: 0px;
}
.posts-expand {
    padding-top: 80px;
}
.posts-expand .post-meta {
    margin: 5px 0px 0px 0px;
}
.post-button {
    margin-top: 0px;
}
// 顶栏宽度
.container .header-inner {
    width: 100%;
}
// 渐变菜带，CSS代码copy自https://githubuniverse.com
.site-meta {
    background-image: linear-gradient(90deg, #F79533 0%, #F37055 15%, #EF4E7B 30%, #A166AB 44%, #5073B8 58%, #1098AD 72%, #07B39B 86%, #6DBA82 100%);
}
// 站点名背景
.brand{
    background-color: rgba(255, 255, 255, 0);
    margin-top: 15px;
    padding: 0px;
}
// 站点名字体
.site-title {
    font-size: 75px;
    font-weight: bold;
    color: rgb(255, 255, 255);
    line-height: 80px;
    letter-spacing: 3px;
}
// 站点子标题
.site-subtitle{
    margin: 0px;
    font-size: 16px;
    letter-spacing: 1px;
    padding-bottom: 3px;
    font-weight: bold;
    color: rgb(255, 255, 255);
    border-bottom-width: 3px;
    border-bottom-style: solid;
    border-bottom-color: rgb(161, 102, 171);
}
// 菜单
.menu {
    text-align: center;
    margin-top: 0px;
    margin-bottom: 0px;
    padding: 5px;
    background-color: rgba(255, 255, 255, 0.75);
    box-shadow: 0px 10px 10px 0px rgba(0, 0, 0, 0.15);
}
// 菜单超链接字体大小
.menu .menu-item a {
    font-size: 15px;
}
// 菜单各项边距
.menu .menu-item {
    margin: 5px 15px;
}
// 菜单超链接样式
.menu .menu-item a:hover {
    border-bottom-color: rgba(161, 102, 171, 0);
}
// 文章
.post {
    margin-bottom: 50px;
    padding: 45px 36px 36px 36px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgb(255, 255, 255);
}
// 文章标题字体
.posts-expand .post-title {
    font-size: 26px;
    font-weight: 700;
}
// 文章标题动态效果
.posts-expand .post-title-link::before {
    background-image: linear-gradient(90deg, #a166ab 0%, #ef4e7b 25%, #f37055 50%, #ef4e7b 75%, #a166ab 100%);
}
// 文章元数据（meta）留白更改
.posts-expand .post-meta {
    margin: 10px 0px 20px 0px;
}
// 文章的描述description
.posts-expand .post-meta .post-description {
    font-style: italic;
    font-size: 14px;
    margin-top: 30px;
    margin-bottom: 0px;
    color: #666;
}
// [Read More]按钮样式
.post-button .btn {
    color: #555 !important;
    background-color: rgb(255, 255, 255);
    border-radius: 3px;
    font-size: 15px;
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    border: none !important;
    transition-property: unset;
    padding: 0px 15px;
}
.post-button .btn:hover {
    color: rgb(255, 255, 255) !important;
    border-radius: 3px;
    font-size: 15px;
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    background-image: linear-gradient(90deg, #a166ab 0%, #ef4e7b 25%, #f37055 50%, #ef4e7b 75%, #a166ab 100%);
}
// 去除在页面文章之间的分割线
.posts-expand .post-eof {
    margin: 0px;
    background-color: rgba(255, 255, 255, 0);
}
// 去除页面底部页码上面的横线
.pagination {
    border: none;
    margin: 0px;
}
// 页面底部页码
.pagination .page-number.current {
    border-radius: 100%;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgba(255, 255, 255, 0.35);
}
.pagination .prev, .pagination .next, .pagination .page-number {
    margin-bottom: 10px;
    border: none;
}
.pagination .space {
    color: rgb(255, 255, 255);
}
// 页面底部页脚
.footer {
    line-height: 1.5;
    background-color: rgba(255, 255, 255, 0.75);
    color: #333;
    border-top-width: 3px;
    border-top-style: solid;
    border-top-color: rgb(161, 102, 171);
    box-shadow: 0px -10px 10px 0px rgba(0, 0, 0, 0.15);
}
// 文章底部的tags
.posts-expand .post-tags a {
    border-bottom: none;
    margin-right: 0px;
    font-size: 13px;
    padding: 0px 5px;
    border-radius: 3px;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0s;
}
.posts-expand .post-tags a:hover {
    background: #eee;
}
// 文章底部留白更改
.post-widgets {
    padding-top: 0px;
}
.post-nav {
    margin-top: 30px;
}
// 文章底部页面跳转
.post-nav-item a {
    color: rgb(80, 115, 184);
    font-weight: bold;
}
.post-nav-item a:hover {
    color: rgb(161, 102, 171);
    font-weight: bold;
}
// 文章底部评论
.comments {
    background-color: rgb(255, 255, 255);
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    margin: 80px 0px 40px 0px;
}
// 超链接样式
a {
    color: rgb(80, 115, 184);
    border-bottom-color: rgb(80, 115, 184);
}
a:hover {
    color: rgb(161, 102, 171);
    border-bottom-color: rgb(161, 102, 171);
}
// 分割线样式
hr {
    margin: 10px 0px 30px 0px;
}
// 文章内标题样式（左边的竖线）
.post-body h2, h3, h4, h5, h6 {
    border-left: 4px solid rgb(161, 102, 171);
    margin-left: -36px;
    padding-left: 32px;
}
// 去掉图片边框
.posts-expand .post-body img {
    border: none;
    padding: 0px;
}
.post-gallery .post-gallery-img img {
    padding: 3px;
}
// 文章``代码块的自定义样式
code {
    margin: 0px 4px;
}
// 文章```代码块顶部样式
.highlight figcaption {
    margin: 0em;
    padding: 0.5em;
    background: #eee;
    border-bottom: 1px solid #e9e9e9;
}
.highlight figcaption a {
    color: rgb(80, 115, 184);
}
// 文章```代码块diff样式
pre .addition {
    background: #e6ffed;
}
pre .deletion {
    background: #ffeef0;
}
// 右下角侧栏按钮样式
.sidebar-toggle {
    right: 10px;
    bottom: 43px;
    background-color: rgba(247, 149, 51, 0.75);
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
}
.page-post-detail .sidebar-toggle-line {
    background: rgb(7, 179, 155);
}
// 右下角返回顶部按钮样式
.back-to-top {
    line-height: 1.5;
    right: 10px;
    padding-right: 5px;
    padding-left: 5px;
    padding-top: 2.5px;
    padding-bottom: 2.5px;
    background-color: rgba(247, 149, 51, 0.75);
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
}
.back-to-top.back-to-top-on {
    bottom: 10px;
}
// 侧栏
.sidebar {
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgba(0, 0, 0, 0.75);
}
.sidebar-inner {
    margin-top: 30px;
}
// 侧栏顶部文字
.sidebar-nav li {
    font-size: 15px;
    font-weight: bold;
    color: rgb(7, 179, 155);
}
.sidebar-nav li:hover {
    color: rgb(161, 102, 171);
}
.sidebar-nav .sidebar-nav-active {
    color: rgb(7, 179, 155);
    border-bottom-color: rgb(161, 102, 171);
    border-bottom-width: 1.5px;
}
.sidebar-nav .sidebar-nav-active:hover {
    color: rgb(7, 179, 155);
}
// 侧栏站点概况行高
.site-overview {
    line-height: 1.3;
}
// 侧栏头像（圆形以及旋转效果）
.site-author-image {
    border: 2px solid rgb(255, 255, 255);
    border-radius: 100%;
    transition: transform 1.0s ease-out;
}
img:hover {
    transform: rotateZ(360deg);
}
.posts-expand .post-body img:hover {
    transform: initial;
}
// 侧栏站点作者名
.site-author-name {
    display: none;
}
// 侧栏站点描述
.site-description {
    letter-spacing: 5px;
    font-size: 15px;
    font-weight: bold;
    margin-top: 15px;
    margin-left: 13px;
    color: rgb(243, 112, 85);
}
// 侧栏站点文章、分类、标签
.site-state {
    line-height: 1.3;
    margin-left: 12px;
}
.site-state-item {
    padding: 0px 15px;
    border-left: 1.5px solid rgb(161, 102, 171);
}
// 侧栏RSS按钮样式
.feed-link {
    margin-top: 15px;
    margin-left: 7px;
}
.feed-link a {
    color: rgb(255, 255, 255);
    border: 1px solid rgb(158, 158, 158) !important;
    border-radius: 15px;
}
.feed-link a:hover {
    background-color: rgb(161, 102, 171);
}
.feed-link a i {
    color: rgb(255, 255, 255);
}
// 侧栏社交链接
.links-of-author {
    margin-top: 0px;
}
// 侧栏友链标题
.links-of-blogroll-title {
    margin-bottom: 10px;
    margin-top: 15px;
    color: rgba(7, 179, 155, 0.75);
    margin-left: 6px;
    font-size: 15px;
    font-weight: bold;
}
// 侧栏超链接样式（友链的样式）
.sidebar a {
    color: #ccc;
    border-bottom: none;
}
.sidebar a:hover {
    color: rgb(255, 255, 255);
}
// 自定义的侧栏时间样式
#days {
    display: block;
    color: rgb(7, 179, 155);
    font-size: 13px;
    margin-top: 15px;
}
// 侧栏目录链接样式
.post-toc ol a {
    color: rgb(7, 179, 155);
    border-bottom: 1px solid rgb(96, 125, 139);
}
.post-toc ol a:hover {
    color: rgb(161, 102, 171);
    border-bottom-color: rgb(161, 102, 171);
}
// 侧栏目录链接样式之当前目录
.post-toc .nav .active > a {
    color: rgb(161, 102, 171);
    border-bottom-color: rgb(161, 102, 171);
}
.post-toc .nav .active > a:hover {
    color: rgb(161, 102, 171);
    border-bottom-color: rgb(161, 102, 171);
}
/* 修侧栏目录bug，如果主题配置文件_config.yml的toc是wrap: true */
.post-toc ol {
    padding: 0px 10px 5px 10px;
}
/* 侧栏目录默认全展开，已注释
.post-toc .nav .nav-child {
    display: block;
}
*/
// 时间轴样式
.posts-collapse {
    margin: 50px 0px;
}
@media (max-width: 1023px) {
    .posts-collapse {
        margin: 50px 20px;
    }
}
// 时间轴左边线条
.posts-collapse::after {
    margin-left: -2px;
    background-image: linear-gradient(180deg,#f79533 0,#f37055 15%,#ef4e7b 30%,#a166ab 44%,#5073b8 58%,#1098ad 72%,#07b39b 86%,#6dba82 100%);
}
// 时间轴左边线条圆点颜色
.posts-collapse .collection-title::before {
    background-color: rgb(255, 255, 255);
}
// 时间轴文章标题左边圆点颜色
.posts-collapse .post-header:hover::before {
    background-color: rgb(161, 102, 171);
}
// 时间轴年份
.posts-collapse .collection-title h1, .posts-collapse .collection-title h2 {
    color: rgb(255, 255, 255);
}
// 时间轴文章标题
.posts-collapse .post-title a {
    color: rgb(80, 115, 184);
}
.posts-collapse .post-title a:hover {
    color: rgb(161, 102, 171);
}
// 时间轴文章标题底部虚线
.posts-collapse .post-header:hover {
    border-bottom-color: rgb(161, 102, 171);
}
// archives页面顶部文字
.page-archive .archive-page-counter {
    color: rgb(255, 255, 255);
}
// archives页面时间轴左边线条第一个圆点颜色
.page-archive .posts-collapse .archive-move-on {
    top: 10px;
    opacity: 1;
    background-color: rgb(255, 255, 255);
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
}
// 分类页面
.post-block.page {
    margin-top: 40px;
}
.category-all-page {
    margin: -80px 50px 40px 50px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgb(255, 255, 255);
    padding: 86px 36px 36px 36px;
}
@media (max-width: 767px) {
    .category-all-page {
        margin: -73px 15px 50px 15px;
    }
    .category-all-page .category-all-title {
        margin-top: -5px;
    }
}
// 标签云页面
.tag-cloud {
    margin: -80px 50px 40px 50px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgb(255, 255, 255);
    padding: 86px 36px 36px 36px;
}
.tag-cloud-title {
    margin-bottom: 15px;
}
@media (max-width: 767px) {
    .tag-cloud {
        margin: -73px 15px 50px 15px;
        padding: 86px 5px 36px 5px;
    }
}
// 自定义的TopX页面样式
#top {
    display: block;
    text-align: center;
    margin: -100px 50px 40px 50px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgb(255, 255, 255);
    padding: 106px 36px 10px 36px;
}
@media (max-width: 767px) {
    #top {
        margin: -93px 15px 50px 15px;
        padding: 96px 10px 0px 10px;
    }
}
// 自定义ABOUT页面的样式
.about-page {
    margin: -80px 0px 60px 0px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: rgb(255, 255, 255);
    padding: 106px 36px 36px 36px;
}
@media (max-width: 767px) {
    .about-page {
        margin: -73px 0px 50px 0px;
        padding: 96px 15px 20px 15px;
    }
}
h2.about-title {
    border-left: none !important;
    margin-left: 0px !important;
    padding-left: 0px !important;
    text-align: center;
    background-image: linear-gradient(90deg, #a166ab 0%, #a166ab 40%, #ef4e7b 45%, #f37055 50%, #ef4e7b 55%, #a166ab 60%, #a166ab 100%);
    background-size: cover;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    user-select: none;
}
// 本地搜索框
.local-search-popup .search-icon, .local-search-popup .popup-btn-close {
    color: rgb(247, 149, 51);
    margin-top: 7px;
}
.local-search-popup .local-search-input-wrapper input {
    padding: 9px 0px;
    height: 21px;
    background-color: rgb(255, 255, 255);
}
.local-search-popup .popup-btn-close {
    border-left: none;
}
// 选中文字部分的样式
::selection {
    background-color: rgb(255, 241, 89);
    color: #555;
}
/* 设置滚动条的样式 */
/* 参考https://segmentfault.com/a/1190000003708894 */
::-webkit-scrollbar {
    width: 5px;
    height: 5px;
}
/* 滚动槽 */
::-webkit-scrollbar-track {
    background: #eee;
}
/* 滚动条滑块 */
::-webkit-scrollbar-thumb {
    border-radius: 5px;
    background-color: #ccc;
}
::-webkit-scrollbar-thumb:hover {
    background-color: rgb(247, 149, 51);
}
// 音乐播放器aplayer
.aplayer {
    font-family: Lato, -apple-system, BlinkMacSystemFont, "PingFang SC", "Hiragino Sans GB", "Heiti SC", STHeiti, "Source Han Sans SC", "Noto Sans CJK SC", "WenQuanYi Micro Hei", "Droid Sans Fallback", "Microsoft YaHei", sans-serif !important;
}
.aplayer-withlrc.aplayer .aplayer-info {
    background-color: rgb(255, 255, 255);
}
// 音乐播放器aplayer歌单
.aplayer .aplayer-list ol {
    background-color: rgb(255, 255, 255);
}
// 修视频播放器dplayer页面全屏的bug
// 已不存在，注释掉了
// .use-motion .post-body {
//     transform: inherit !important;
// }
// 自定义emoji样式
img#github-emoji {
    margin: 0px;
    padding: 0px;
    display: inline !important;
    vertical-align: text-bottom;
    border: none;
    cursor: text;
    box-shadow: none;
}
// 文章超链接样式（为emoji特设）
.post-body a {
    color: rgb(80, 115, 184);
    border-bottom: none;
    text-decoration: underline;
}
.post-body a:hover {
    color: rgb(161, 102, 171);
    border-bottom: none;
    text-decoration: underline;
}
// 标签云页面超链接样式（为emoji特设）
.tag-cloud a {
    border-bottom: 1px solid rgb(80, 115, 184);
    text-decoration: none;
}
.tag-cloud a:hover {
    border-bottom: 1px solid rgb(161, 102, 171);
    text-decoration: none;
}
// 文章元数据中categories的样式（为emoji特设）
a.categories {
    color: rgb(80, 115, 184);
    border-bottom: none;
    text-decoration: underline;
}
a.categories:hover {
    color: rgb(161, 102, 171);
    border-bottom: none;
    text-decoration: underline;
}
// tabs标签（为emoji特设）
.post-body .tabs ul.nav-tabs li.tab a {
    text-decoration: none;
}
// 图片下方标题设置（为emoji特设）
a.fancybox{
    text-decoration: none !important;
}
// 按钮样式（为emoji特设）
.btn {
    color: #fff !important;
    text-decoration: none !important;
    border: 2px solid #222 !important;
}
.btn:hover {
    color: #222 !important;
}
// 自定义的页脚微信订阅号样式
.weixin-box {
    position: absolute;
    bottom: 43px;
    left: 10px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
}
.weixin-menu {
    position: relative;
    height: 24px;
    width: 24px;
    cursor: pointer;
    background: url(https://微信的logo.svg);
    background-size: 24px 24px;
}
.weixin-hover {
    position: absolute;
    bottom: 0px;
    left: 0px;
    height: 0px;
    width: 0px;
    border-radius: 3px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    background: url(https://二维码.svg);
    background-color: #fff;
    background-repeat: no-repeat;
    background-size: 150px 150px;
    transition: all 0.35s ease-in-out;
    z-index: 1024;
    opacity: 0;
}
.weixin-menu:hover .weixin-hover {
    bottom: 24px;
    left: 24px;
    height: 170px;
    width: 150px;
    opacity: 1;
}
.weixin-description {
    opacity: 0;
    position: absolute;
    bottom: 3%;
    left: 5%;
    right: 5%;
    font-size: 12px;
    transition: all 0.35s cubic-bezier(1, 0, 0, 1);
}
.weixin-menu:hover .weixin-description {
    opacity: 1;
}
// 自定义页脚跳动的心样式
@keyframes heartAnimate {
    0%,100%{transform:scale(1);}
    10%,30%{transform:scale(0.9);}
    20%,40%,60%,80%{transform:scale(1.1);}
    50%,70%{transform:scale(1.1);}
}
#heart {
    animation: heartAnimate 1.33s ease-in-out infinite;
}
.with-love {
    color: rgb(255, 113, 168);
}
// 自定义特别的样式
h2.love {
    border-left: none;
    color: rgb(255, 113, 168);
    -webkit-text-fill-color: unset;
}
// 自定义的引用样式
blockquote.question {
    color: #555;
    border-left: 4px solid rgb(16, 152, 173);
    background-color: rgb(227, 242, 253);
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    margin-bottom: 20px;
}
// 自定义的数字块
span#inline-toc {
    display: inline-block;
    border-radius: 80% 100% 90% 20%;
    background-color: rgb(227, 242, 253);
    color: #555;
    padding: 0.05em 0.4em;
    margin: 2px 5px 2px 0px;
    line-height: 1.5;
}
// 自定义的文章摘要图片样式
img.img-topic {
    width: 100%;
}
// 自定义的文章置顶样式
.post-sticky-flag {
    font-size: inherit;
    float: left;
    color: rgb(0, 0, 0);
    cursor: help;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0s;
}
.post-sticky-flag:hover {
    color: #07b39b;
}
// 自定义替代description的样式
p.description{
    text-align: center;
    font-size: 14px;
    font-style: italic;
    color: #666;
    margin-top: 30px;
}
code.description {
    padding: 1px 1px 1px 1px;
    margin: 0px 1px 0px 4px;
}
// 移动端样式
@media (max-width: 1023px) {
    .container {
        background-color: rgba(0, 0, 0, 0);
    }
    .sidebar {
        box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
    }
    .feed-link {
        display: none !important;
    }
}
@media (max-width: 767px) {
    .main {
        padding-bottom: 120px;
    }
    .posts-expand {
        margin: 0px;
        padding-top: 70px;
    }
    .posts-expand .post-title {
        font-size: 22px;
    }
    .posts-expand .post-meta {
        font-size: 10px;
    }
    .post {
        margin-bottom: 30px !important;
        padding: 20px 15px 15px 15px !important;
    }
    .post-body h2, h3, h4, h5, h6 {
        margin-left: -15px;
        padding-left: 11px;
    }
    .posts-expand .post-tags {
        margin-top: 10px;
    }
    .post-widgets {
        margin-top: 10px;
    }
    .comments {
        margin: 40px 0px 40px 0px !important;
    }
    .footer {
        box-shadow: 0px -5px 10px 0px rgba(0, 0, 0, 0.5);
    }
}
.sidebar-active #sidebar-dimmer {
    opacity: 0;
}
// 移动端左上角菜单按钮
.site-nav-toggle {
    top: 35px;
    left: 10px;
}
.btn-bar {
    background-color: rgb(255, 255, 255);
}
// 移动端菜单
@media (max-width: 767px) {
    .menu {
        text-align: center;
        box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.5);
    }
    .site-nav {
        top: initial;
        background-color: rgba(255, 255, 255, 0.75);
        border-top: none;
        border-bottom: none;
        position: relative;
        z-index: 1024;
    }
}
// 移动端顶部
@media (max-width: 767px) {
    .site-title {
        font-size: 70px !important;
        letter-spacing: 0px !important;
    }
    .site-subtitle{
        letter-spacing: 0px !important;
        padding-bottom: 0px !important;
    }
    .site-meta {
        box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.5);
    }
    .menu .menu-item {
        margin: 0px 10px !important;
    }
}
```

### 修改字体

优化了这么多，但还有一个最影响博客形象和阅读体验的项没有优化，瓦特？字体！

文章字体大小可以编辑：

```
文件位置：~/blog/themes/next/source/css/_variables/base.styl$font-size-base           = 16px
```

，推荐阅读：

1. [Web中文字体排版指南](https://www.voyax.me/posts/59710/)
2. [Web字体的选择和运用](https://blog.coding.net/blog/Web-Fonts)
3. [如何优雅的选择默认字体(font-family)](https://segmentfault.com/a/1190000006110417)
4. [中文字体网页开发指南](http://www.ruanyifeng.com/blog/2014/07/chinese_fonts.html)
5. [在Web内容中使用系统字体](https://csspod.com/using-the-system-font-in-web-content/)

首先对于汉字来说，因为其字体库太大，通常都是调用本地中文字体库。然而，不同设备有不同默认中文字体和中文字体库，想要尽可能在不同设备上有较好的显示效果，就要在调用不同设备的本地字体库中显示效果较好的中文字体。

下面附上我的供大家参考：

```
文件位置：~/blog/themes/next/source/css/_variables/base.styl// Font families.
$font-family-chinese      = -apple-system, BlinkMacSystemFont, "PingFang SC", "Hiragino Sans GB", "Heiti SC", "STHeiti", "Source Han Sans SC", "Noto Sans CJK SC", "WenQuanYi Micro Hei", "Droid Sans Fallback", "Microsoft YaHei"

$font-family-base         = $font-family-chinese, sans-serif
$font-family-base         = get_font_family('global'), $font-family-chinese, sans-serif if get_font_family('global')

$font-family-logo         = $font-family-base
$font-family-logo         = get_font_family('logo'), $font-family-base if get_font_family('logo')

$font-family-headings     = $font-family-base
$font-family-headings     = get_font_family('headings'), $font-family-base if get_font_family('headings')

$font-family-posts        = $font-family-base
$font-family-posts        = get_font_family('posts'), $font-family-base if get_font_family('posts')

$font-family-monospace    = $font-family-chinese, monospace
$font-family-monospace    = Menlo, Monaco, Consolas, get_font_family('codes'), $font-family-chinese, monospace if get_font_family('codes')
```

注意：要想 NexT 主题的简体中文字体配置生效，站点配置文件中的 language 必须为 zh-CN。

然后对于英文字体，因为其字体库很小，所以想要个性化就简单多了。首先去 [Google Fonts](https://fonts.google.com/) 找自己喜欢的英文字体，然后编辑主题配置文件，可以查看一下 [NexT 官方文档](http://theme-next.iissnan.com/theme-settings.html#fonts-customization)。

下面附上我的供大家参考：

```
文件位置：~/blog/themes/next/_config.ymlfont:
  enable: true

  # Uri of fonts host. E.g. //fonts.googleapis.com (Default)
  # 亲测这个可用，如果不可用，自己搜索 [Google 字体 国内镜像]，找个能用的就行
  host: https://fonts.loli.net

  # Global font settings used on <body> element.
  global:
    # external: true will load this font family from host.
    external: true
    family: Lato

  # Font settings for Headlines (h1, h2, h3, h4, h5, h6)
  # Fallback to `global` font settings.
  headings:
    external: true
    family: Roboto Slab

  # Font settings for posts
  # Fallback to `global` font settings.
  posts:
    external: true
    family:

  # Font settings for Logo
  # Fallback to `global` font settings.
  # The `size` option use `px` as unit
  logo:
    external: true
    family:
    size:

  # Font settings for <code> and code blocks.
  codes:
    external: true
    family: Roboto Mono
    size:
```

## 9. 进阶 高级功能配置

这些功能的配置，大部分都要修改 NexT 主题的一些文件，所以`git pull`升级主题的时候，会比较麻烦，解决方法见

### 参考的文章

更多如外挂一样的功能配置，就直接贴大佬的文章了，哪些功能自己喜欢，按照大佬的教程来配置就行。不过也有坑，比如有些功能（超链接样式、侧栏头像圆形并旋转）可以直接通过在`custom.styl`添加 CSS 代码实现，无需更改其它文件！

1. [hexo高阶教程next主题优化](http://cherryblog.site/Hexo-high-level-tutorialcloudmusic,bg-customthemes-statistical.html)
2. [hexo的next主题个性化教程:打造炫酷网站](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
3. [Hexo搭建博客的个性化设置](http://www.dingxuewen.com/categories/Site/)

第一篇文章内有 NexT 主题的文件目录说明，这对自己自定义博客有很大帮助。

### 更改上一篇，下一篇的顺序

进入一篇文章，在文章底部，有上下篇的链接（< >），但是点 > 发现进入的是页面中的的上面那篇文章，与操作习惯不符，别扭。

我猜这是从时间角度设计的，> 英语叫 next，而 next 是更新的。不过别扭就改成习惯的好了，从空间位置角度设计。

方法就是修改文件：

```
文件位置：~/blog/themes/next/layout/_macro/post.swig{% if not is_index and (post.prev or post.next) %}
  <div class="post-nav">
    <div class="post-nav-next post-nav-item">
-      {% if post.next %}
+      {% if post.prev %}
-        <a href="{{ url_for(post.next.path) }}" rel="next" title="{{ post.next.title }}">
+        <a href="{{ url_for(post.prev.path) }}" rel="prev" title="{{ post.prev.title }}">
-          <i class="fa fa-chevron-left"></i> {{ post.next.title }}
+          <i class="fa fa-chevron-left"></i> {{ post.prev.title }}
        </a>
      {% endif %}
    </div>

    <span class="post-nav-divider"></span>

    <div class="post-nav-prev post-nav-item">
-      {% if post.prev %}
+      {% if post.next %}
-        <a href="{{ url_for(post.prev.path) }}" rel="prev" title="{{ post.prev.title }}">
+        <a href="{{ url_for(post.next.path) }}" rel="next" title="{{ post.next.title }}">
-          {{ post.prev.title }} <i class="fa fa-chevron-right"></i>
+          {{ post.next.title }} <i class="fa fa-chevron-right"></i>
        </a>
      {% endif %}
    </div>
  </div>
{% endif %}
```

### 移动端显示 back-to-top 按钮和侧栏

今天更新一下 NexT 主题，发现已经添加这功能，前提是主题的设计模版是 Muse 或 Mist，然后可以直接在主题配置文件中配置：

```
文件位置：~/blog/themes/next/_config.yml# Enable sidebar on narrow view
onmobile: true
```

如果你发现你的主题配置文件`_config.yml`中没有这段内容，

#### 搜索引擎

搜索引擎是互联网上寻找资源的重要手段，而要让别人能够在搜索结果中看到自己的博客文章链接，就必须让搜索引擎收录，怎么操作呢？

直接参考[这篇文章](http://www.ehcoo.com/seo.html)，学会使用站长工具抓取自己的网页，然后请求搜索引擎收录。博主也是刚接触不久，不太懂，但推荐提交次数尽量多，而且每天尽量都提交一次。

#### 间接影响

另外，SEO 固然重要，但不要小看另一种影响，相比搜索引擎，这种可以称之为间接影响。

这篇文章是一篇技术性的文章，而技术人员经常会用 Google，所以对这篇文章的浏览量，搜索引擎的功劳较大。但是，如果是其他的文章，比如一首诗，那么直接通过 Google 访问的读者几乎没有，那读者哪来？从其它文章上的读者「流」过来的。因为读者浏览着的不是一篇文章，而是一个博客。

而想让博客上的几乎不可能被 Google 的一首诗被浏览，就要这样间接拉读者了，可以称之「引流」。首先对博客上的每篇文章来说，肯定是读者花在自己博客的时间越长，被读到的可能性越大。这就意味着你要尽可能把用户留在自己的博客上，怎么留？

1. 博客要装饰好
2. 文章质量要高

读者的第二印象是博客的界面，如果界面够特别，也许马上就被加入了书签。第三印象是文章内容，这其实更加重要，如果文章质量很高，那么读者肯定不会让这么好的一篇文章消失在自己的记忆中，即使界面不咋地。第一印象？加载速度，试想点开半天还是空白，那么肯定马上关了。

如果做到上面三点，那么就算好不容易「骗」到一个 Google 浏览量，但是这个读者马上被博客和文章惊呆了，看完文章后，这读者心里美滋滋，认为这么好的文章（博客）必须分享啊

#### 知识平台

直接或间接因为 Google 这样的搜索引擎而来的读者，绝大部分都是技术人员，而他们只希望尽快解决自己的技术问题，这也是他们的目的，这就意味着博客上的一首诗还是很难被欣赏。

而要想照亮他人，他人必须要能懂自己的文章，这样也才可能有更强的交互——评论。所以为了不浪费自己的光能，能把自己的光能完完整整的贡献给文明，那就必须也让一首诗也有评论，怎么做呢？让读者的类型多样化，不限于技术人员。

#### 谷歌分析

你怎么知道自己推广的效果？你怎么知道有没有人看了自己的博客？哪篇文章最受欢迎？此时有没有人正浏览着自己的博客？自己的文章有没有被引用？这时最常用的就是强大免费的 [Google Analytics](https://analytics.google.com/)，推荐博客建好后，就立即使用。

如何使用？请务必自备梯子查看 [Google 官方的教程](https://analytics.google.com/analytics/academy/course/6)，开始使用后一定要按照里面的设置，先添加多份 view（数据视图）。

**ATTENTION：**虽然有个复制 view 选项，但由 Google Analytics（分析）帮助中的[具体复制内容](https://support.google.com/analytics/answer/3256366?hl=zh-Hans)再加上我的实践，发现（用我这个外行人的话来说）：复制 view 时只会复制它的相关配置，不会复制数据！所以请使用后立即按照官方教程中的方式添加 view ！

博客刚起步，博客的大部分访客都是自己，所以必须添加一个 filter（过滤器），过滤掉自己的访问（比如：过滤掉自己科学上网服务器的 IP）。

**ATTENTION：**由 Google Analytics（分析）的[工作原理](https://support.google.com/analytics/answer/6383007)可知，filter 是在数据处理时生效的（如上图），也就是说添加 filter 后只能过滤添加它之后的数据，而无法过滤添加它之前的数据（处理后），而 view 是利用处理后的数据生成的，所以要想去除自己访问的影响（在 view 中看不到自己的访问），请添加 view 之后就立即添加 filter ！

### 时间轴页面的年份分隔

在 Archives（归档）页面，文章之间有年份分隔，而某一个 category 和 tag 的时间轴页面却没有。怎么办呢？修改两个文件，加代码即可

#### category

加到哪？要加两个位置：

```
文件位置：~/blog/themes/next/layout/category.swig    {% for post in page.posts %}
              位置A
      {{ post_template.render(post) }}
    {% endfor %}
.
.
.（省略好多行）
.
.
位置B（没错最后面）
```

加什么？绿色的自己看着加：

```
文件位置：~/blog/themes/next/layout/category.swig我不要 + 号！    {% for post in page.posts %}
+
+      {# Show year #}
+      {% set year %}
+      {% set post.year = date(post.date, 'YYYY') %}
+
+      {% if post.year !== year %}
+        {% set year = post.year %}
+        <div class="collection-title">
+          <h2 class="archive-year motion-element" id="archive-year-{{ year }}">{{ year }}</h2>
+        </div>
+      {% endif %}
+      {# endshow #}
+
      {{ post_template.render(post) }}
    {% endfor %}
.
.
.（省略好多行）
.
.
+{% block script_extra %}
+  {% if theme.use_motion %}
+    <script type="text/javascript" id="motion.page.archive">
+      $('.archive-year').velocity('transition.slideLeftIn');
+    </script>
+  {% endif %}
+{% endblock %}
```

#### tag

文件位置：`~/blog/themes/next/layout/tag.swig`，其它与的 category 修改完全一样。

### 文章底部加上评分小星星

淘宝买东西，作为消费者的我们，看评价很重要。现在作为博主，写了一篇文章，很期待读者的反馈。而与淘宝一样，确认收货后，相比评论，更愿意五星好评。那么博客文章怎么加上呢？

首先打开主题配置文件：

```
文件位置：~/blog/themes/next/_config.yml# Star rating support to each article.
# To get your ID visit https://widgetpack.com
rating:
  enable: true
  id:     
  color: f79533
```

先去注释中的网站，首页点 Rating，然后注册个帐号，填一下自己博客的信息，左上角有个 ID，填进主题配置文件中就行，`color`改成自己喜欢的即可。另：

1. 可以配置评分方式，侧栏 > Rating > Setting，建议用 IP address 或 Device(cookie)，免登录，毕竟 Socials 里面的选项几乎都被墙，不适合国内网络环境。
2. 建议 侧栏 > Site > Setting 中勾选 Private 选项。
3. 上面两步勾选后别忘了点击页面右下方的 **SAVE SETTING** 绿色按钮保存。

如果感觉上下留白太多，咋整？浏览器 F12 找元素，调成自己喜欢的值，然后 Copy 到`custom.styl`即可，

首先打开文件：

```
文件位置：~/blog/themes/next/layout/_macro/post.swig        {% if theme.rating.enable %}
          <div class="wp_rating">
+            <div style="color: rgba(0, 0, 0, 0.75); font-size:13px; letter-spacing:3px">(&gt;看完记得五星好评哦亲&lt;)</div>
            <div id="wpac-rating"></div>
          </div>
        {% endif %}
```

然后 Ctrl + F 搜索`rating`，找到这段，对比我给出的，在绿色这行所示的位置，加上自己想要的说明和样式即可。

### 侧栏加入已运行的时间

首先要加入下面代码：

```
文件位置：~/blog/themes/next/layout/_custom/sidebar.swig<div id="days"></div>
<script>
function show_date_time(){
window.setTimeout("show_date_time()", 1000);
BirthDay=new Date("05/27/2017 15:13:14");
today=new Date();
timeold=(today.getTime()-BirthDay.getTime());
sectimeold=timeold/1000
secondsold=Math.floor(sectimeold);
msPerDay=24*60*60*1000
e_daysold=timeold/msPerDay
daysold=Math.floor(e_daysold);
e_hrsold=(e_daysold-daysold)*24;
hrsold=setzero(Math.floor(e_hrsold));
e_minsold=(e_hrsold-hrsold)*60;
minsold=setzero(Math.floor((e_hrsold-hrsold)*60));
seconds=setzero(Math.floor((e_minsold-minsold)*60));
document.getElementById('days').innerHTML="已运行 "+daysold+" 天 "+hrsold+" 小时 "+minsold+" 分 "+seconds+" 秒";
}
function setzero(i){
if (i<10)
{i="0" + i};
return i;
}
show_date_time();
</script>
```

上面`Date`的值记得改为你自己的，且按上面格式，然后修改：

```
文件位置：~/blog/themes/next/layout/_macro/sidebar.swig        {# Blogroll #}
        {% if theme.links %}
          <div class="links-of-blogroll motion-element {{ "links-of-blogroll-" + theme.links_layout | default('inline') }}">
            <div class="links-of-blogroll-title">
              <i class="fa  fa-fw fa-{{ theme.links_icon | default('globe') | lower }}"></i>
              {{ theme.links_title }}&nbsp;
              <i class="fa  fa-fw fa-{{ theme.links_icon | default('globe') | lower }}"></i>
            </div>
            <ul class="links-of-blogroll-list">
              {% for name, link in theme.links %}
                <li class="links-of-blogroll-item">
                  <a href="{{ link }}" title="{{ name }}" target="_blank">{{ name }}</a>
                </li>
              {% endfor %}
            </ul>
+        {% include '../_custom/sidebar.swig' %}
          </div>
         {% endif %}

-        {% include '../_custom/sidebar.swig' %}
```

这样就可以了！当然，要是不喜欢颜色，感觉不好看，就可以在上文所提的`custom.styl`加入：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的侧栏时间样式
#days {
    display: block;
    color: rgb(7, 179, 155);
    font-size: 13px;
    margin-top: 15px;
}
```

里面的值 F12 调成自己喜欢的，然后更改即可。要是不想放在侧栏，想放在页脚

### 添加 TopX 页面

博客已有的分类，如 categories 和 tags，都是基于博主的，那么有没有一种分类是基于读者的呢？有，一种是搜索，另一种就是这里的文章阅读量排行榜。

前提是在主题配置文件中配置了 leancloud_visitors，

首先新建页面：

```
所在目录：~/blog/hexo new page "top"
```

然后在主题配置文件中加上菜单 top 和它的 icon：

```
文件位置：~/blog/themes/next/_config.ymlmenu:
  top: /top/ || signal
```

接着在语言翻译文件中加上菜单 top：

注意：如果你的站点配置文件中的 languages 写的不是 zh-CN，那么这里请更改相应语言配置文件。

```
文件位置：~/blog/themes/next/languages/zh_Hans.ymlmenu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益404
  top: TopX /* 可以不为 TopX，随便取 */
```

最后，编辑第一步新建页面生成的文件：

```
文件位置：~/blog/source/top/index.md---
title: TopX /* 可以不为 TopX，随便取 */
comments: false
keywords: top,文章阅读量排行榜
description: 博客文章阅读量排行榜
---
<div id="top"></div>
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script>AV.initialize("app_id", "app_key");</script>
<script type="text/javascript">
  var time=0
  var title=""
  var url=""
  var query = new AV.Query('Counter');
  query.notEqualTo('id',0);
  query.descending('time');
  query.limit(1000);
  query.find().then(function (todo) {
    for (var i=0;i<1000;i++){
      var result=todo[i].attributes;
      time=result.time;
      title=result.title;
      url=result.url;
      var content="<a href='"+"https://reuixiy.github.io"+url+"'>"+title+"</a>"+"<br />"+"<font color='#555'>"+"阅读次数："+time+"</font>"+"<br /><br />";
      document.getElementById("top").innerHTML+=content
    }
  }, function (error) {
    console.log("error");
  });
</script>

<style>.post-description { display: none; }</style>
```

必须将里面的里面的`app_id`和`app_key`替换为你的主题配置文件中的值，必须替换里面博客的链接，`1000`是显示文章的数量，其它可以自己看情况更改。

最后，修改样式可以在`custom.styl`中加入自定义代码

Okay! 完成了！不过还有几点需要注意：

1. 如果在 设置 > 安全中心 中，设置了 Web 安全域名，但没有将`http://localhost:4000`加入，那么本地调试将看不到，可以先将之加入，调试完后删除。
2. 如果你发现文章标题显示不对，这是由于更改过文章标题导致的，在 存储 > Counter 双击`title`修改即可。

### 利用 gulp 压缩代码

右键查看网页源代码发现有大量留白，咋整？利用 gulp。

首先任意目录全局安装：

```
npm install gulp -g
```

然后到站点文件夹根目录：

```
所在目录：~/blog/npm install gulp gulp-minify-css gulp-htmlmin gulp-htmlclean --save
```

接着在站点文件夹根目录新建 gulpfile.js：

```
文件位置：~/blog/gulpfile.jsvar gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss())
        .pipe(gulp.dest('./public'))
});
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
    .pipe(htmlclean())
    .pipe(htmlmin({
         removeComments: true,
         minifyJS: true,
         minifyCSS: true,
         minifyURLs: true
    }))
    .pipe(gulp.dest('./public'))
});
gulp.task('default', ['minify-html', 'minify-css']);
```

最后部署到 GitHub Pages 上查看效果：

```
所在目录：~/blog/hexo clean && hexo g && gulp && hexo d
```

我没有压缩 JavaScript，因为我发现会报错，实际也并不需要，因为大部分 JavaScript 都已压缩过。

这里的这段代码执行 gulp 后不支持 hexo s 本地调试，记得在哪看过解决方法，需要的自己 Google。

另外，可能会产生一些奇怪的 bugs，没看到最好，要是看到了的话就自己解决吧～[逃……]

### 让页脚的心跳动起来

世界上有一种伟大的力量，它的名字无人不晓，就是……爱～

更新 NexT 主题后，发现默认的`icon`变成了（user），不过这可阻挡不了爱的力量！

首先编辑主题配置文件：

```
文件位置：~/blog/themes/next/_config.ymlfooter:
-  icon: user
+  icon: heart
```

然后编辑：

```
文件位置：~/blog/themes/next/layout/_partials/footer.swig- <span class="with-love">
+ <span class="with-love" id="heart">
```

接着编辑`custom.styl`，加入：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义页脚跳动的心样式
@keyframes heartAnimate {
    0%,100%{transform:scale(1);}
    10%,30%{transform:scale(0.9);}
    20%,40%,60%,80%{transform:scale(1.1);}
    50%,70%{transform:scale(1.1);}
}
#heart {
    animation: heartAnimate 1.33s ease-in-out infinite;
}
.with-love {
    color: rgb(255, 113, 168);
}
```

color 的值可以改成你自己喜欢的，灵感来自 [DIYgod](https://diygod.me/) 大佬的博客，CSS 代码参考[这篇文章](http://www.jianshu.com/p/73b46c376188)。

### 页脚加上微信二维码

主题默认的微信订阅个人感觉不美观，看到很多网站都是在页脚有个微信的 Logo，然后鼠标移动到上面便会显示二维码，这样感觉很棒。

首先编辑文件，在文件最后加上下面代码：

```
文件位置：~/blog/themes/next/layout/_partials/footer.swig<div class="weixin-box">
  <div class="weixin-menu">
    <div class="weixin-hover">
      <div class="weixin-description">微信扫一扫，订阅本博客</div>
    </div>
  </div>
</div>
```

然后编辑`custom.styl`，加入：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的页脚微信订阅号样式
.weixin-box {
    position: absolute;
    bottom: 43px;
    left: 10px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
}
.weixin-menu {
    position: relative;
    height: 24px;
    width: 24px;
    cursor: pointer;
    background: url(https://微信的logo.svg);
    background-size: 24px 24px;
}
.weixin-hover {
    position: absolute;
    bottom: 0px;
    left: 0px;
    height: 0px;
    width: 0px;
    border-radius: 3px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    background: url(https://二维码.svg);
    background-color: #fff;
    background-repeat: no-repeat;
    background-size: 150px 150px;
    transition: all 0.35s ease-in-out;
    z-index: 1024;
    opacity: 0;
}
.weixin-menu:hover .weixin-hover {
    bottom: 24px;
    left: 24px;
    height: 170px;
    width: 150px;
    opacity: 1;
}
.weixin-description {
    opacity: 0;
    position: absolute;
    bottom: 3%;
    left: 5%;
    right: 5%;
    font-size: 12px;
    transition: all 0.35s cubic-bezier(1, 0, 0, 1);
}
.weixin-menu:hover .weixin-description {
    opacity: 1;
}
```

图片务必用矢量图 svg 格式，否则手机上显示效果很差，其它的自己看情况更改。



### 更改标签云（tagcloud）的颜色

如果你和我一样个性化了博客的整体布局颜色，那么默认的标签云页面可能看起来很丑，怎么更改？

修改下文件：

```
文件位置：~/blog/themes/next/layout/page.swig{{ tagcloud({min_font: 13, max_font: 31, amount: 1000, color: true, start_color: '#9733EE', end_color: '#FF512F'}) }}
```

修改对应参数值即可，参数说明见 [Hexo 官方文档](https://hexo.io/zh-cn/docs/helpers.html#tagcloud)，颜色可以参考[这个网站](https://uigradients.com/)，自己去纠结……

### 点击侧栏头像回到博客首页

不知道为什么，我看到侧栏头像的第一反应是点击，然后心理预期会跳到博客首页，可惜也仅是预期，那么开始动手吧～

首先要在主题配置文件中配置好，比如我把头像`avatar.gif`放在`~/blog/source/uploads/`下，则：

```
文件位置：~/blog/themes/next/_config.yml# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
-#avatar: /images/avatar.gif
+avatar: /uploads/avatar.gif
```

然后编辑文件：

```
文件位置：~/blog/themes/next/layout/_macro/sidebar.swig+        <a href="/">
          <img class="site-author-image" itemprop="image"
               src="{{ url_for( theme.avatar | default(theme.images + '/avatar.gif') ) }}"
               alt="{{ theme.author }}" />
+        </a>
```

最后就 OK 了～

### 文章摘要图片

俗话说：「图文并茂」，现实中用笔书写文章实现起来比较困难，但在博客上可以很轻松实现。

首先，文章摘要（excerpt）是指每篇文章在页面（page）上显示的那部分内容，也就是 [Read More] 之前的文章内容。由于它会展示在页面，因此在每篇文章的文章摘要中加一张图片，页面看起来就很美观。

但是有时候可能会出现一个问题：你想从文章中选一张图片作为文章摘要图片，而这张图片由于写作要求，必须添加在文章的末尾，这样点 [Read More] 查看文章时，这张图片就会重复出现了。咋办？

前提是在主题配置文件中：

```
文件位置：~/blog/themes/next/_config.ymlexcerpt_description: false

auto_excerpt:
  enable: false
```

首先加代码：

```
文件位置：~/blog/themes/next/layout/_macro/post.swig      {% if is_index %}
        {% if post.description and theme.excerpt_description %}
          {{ post.description }}
          <!--noindex-->
          <div class="post-button text-center">
            <a class="btn" href="{{ url_for(post.path) }}">
              {{ __('post.read_more') }} &raquo;
            </a>
          </div>
          <!--/noindex-->
        {% elif post.excerpt  %}
          {{ post.excerpt }}
+          
+        {% if post.image %}
+        <div class="out-img-topic">
+          <img src={{ post.image }} class="img-topic" />
+        </div>
+        {% endif %}
+          
          <!--noindex-->
          <div class="post-button text-center">
            <a class="btn" href="{{ url_for(post.path) }}{% if theme.scroll_to_more %}#{{ __('post.more') }}{% endif %}" rel="contents">
              {{ __('post.read_more') }} &raquo;
            </a>
          </div>
          <!--/noindex-->
```

为了防止有的图片宽度不够导致风格不够统一，页面不美观，需要在`custom.styl`中加入：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的文章摘要图片样式
img.img-topic {
    width: 100%;
}
```

最后编辑有这需求的相关文章时，在`Front-matter`（文件最上方以`---`分隔的区域）加上一行：

```
image: url
```

`url`即图片的链接地址～

参考：

1. issue：<https://github.com/iissnan/hexo-theme-next/issues/1190>
2. 文章：[http://wellliu.com/2016/12/30/【转】Blog摘要配图/](http://wellliu.com/2016/12/30/%E3%80%90%E8%BD%AC%E3%80%91Blog%E6%91%98%E8%A6%81%E9%85%8D%E5%9B%BE/)

### 文章置顶

由于博客的首页可能是被浏览最多的页面，所以首页的前几篇文章被阅读的可能性比较大。可以利用这个特点，通过将自己认为重要的文章放在首页，从而让重要的文章被阅读的可能性增大。

但是，默认的排序只有一个维度——时间，两种选择——正序和倒序，这就造成自己的得意之作被「埋没」了，怎么办呢，如何实现文章的置顶？

NexT 主题以前有过这个功能，然而由于一些 bugs（[issue](https://github.com/iissnan/hexo-theme-next/issues/415)）被去掉了。不过在这个丰富的 issue 中，我自己摸索出了一种解决方法，参考了 issue 中的那篇[文章](http://www.netcan666.com/2015/11/22/%E8%A7%A3%E5%86%B3Hexo%E7%BD%AE%E9%A1%B6%E9%97%AE%E9%A2%98/)。

首先移除默认安装的插件：

```
所在目录：~/blog/npm uninstall hexo-generator-index --save
```

然后安装新插件：

```
所在目录：~/blog/npm install hexo-generator-index-pin-top --save
```

最后编辑有这需求的相关文章时，在`Front-matter`（文件最上方以`---`分隔的区域）加上一行：

```
top: true
```

然后就行了，如果你置顶了多篇，怎么控制顺序呢？设置`top`的值（大的在前面），比如：

```
# Post a.md
title: a
top: 1

# Post b.md
title: b
top: 10
```

那么文章 b 便会显示在文章 a 的前面。

还好 NexT 原有的置顶功能有考虑到这个，且置顶的样式没有被移除，所以可以直接利用，编辑文件：

```
文件位置：~/blog/node_modules/hexo-generator-index-pin-top/lib/generator.js'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.sticky && b.sticky) { // 两篇文章sticky都有定义
            if(a.sticky == b.sticky) return b.date - a.date; // 若sticky值一样则按照文章日期降序排
            else return b.sticky - a.sticky; // 否则按照sticky值降序排
        }
        else if(a.sticky && !b.sticky) { // 以下是只有一篇文章sticky有定义，那么将有sticky的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.sticky && b.sticky) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

也就是将插件的`top`全部替换为 NexT 原有的`sticky`，然后将`Front-matter`中的`top`替换为`sticky`，就能调用 NexT 主题原有的置顶样式了。

最后可以自定义一下样式：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的文章置顶样式
.post-sticky-flag {
    font-size: inherit;
    float: left;
    color: rgb(0, 0, 0);
    cursor: help;
    transition-duration: 0.2s;
    transition-timing-function: ease-in-out;
    transition-delay: 0s;
}
.post-sticky-flag:hover {
    color: #07b39b;
}
```

已发现的 bug：新安装的插件无法实现站点配置文件中`order_by: date`，即文章按时间从旧到新排列的配置，也就意味着文章只能按默认的时间从新到旧排列。

### 背景图片

通过 jquery-backstretch，具体操作呢，编辑文件：

```
文件位置：~/blog/themes/next/layout/_layout.swig+  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-backstretch/2.0.4/jquery.backstretch.min.js"></script>
+  <script>
+  $("body").backstretch("https://背景图.jpg");
+  </script>
</body>
```

加入到文件最后面`</body>`前面即可。

你可以浏览器按 F12 查看我的页面，就可以在`</body>`前发现。

幻灯片等更多用法请自行查看 GitHub 上的 [README](https://github.com/jquery-backstretch/jquery-backstretch)。

### 动态效果

可以在主题配置文件`_config.yml`里的`motion`中配置，但是如果你和我一样更改了博客的背景色，可能不能达到很好的效果，怎么办呢？参考[这里](https://github.com/iissnan/hexo-theme-next/pull/1829/files)，修改下面两个文件的相应内容。

1. ~/blog/themes/next/source/css/_common/components/post/post.styl
2. ~/blog/themes/next/source/js/src/motion.js

### 相关/热门/推荐文章

<https://reuixiy.github.io/technology/computer/computer-aided-art/2018/05/09/related-popular-recommended-posts.html>

### MathJax 的静态显示（svg）

<https://reuixiy.github.io/technology/computer/computer-aided-art/2018/05/16/hexo-mathjax-svg.html>

### 加速 Hexo 博客

<https://reuixiy.github.io/technology/computer/computer-aided-art/2018/05/30/speed-up-hexo.html>

### 萌萌哒十二生

<https://reuixiy.github.io/technology/computer/computer-aided-art/2018/08/27/add-chinese-zodiac-to-next.html>

## 进阶 写出优雅文章

博客搭好了，只能说有个漂亮的外壳，内容丰富且颜值高的文章才是真正的精华。文章内容只能靠自己，不过这里教你几招提高文章颜值的方法。

写文章前请先查看 Hexo 官方文档之[写作](https://hexo.io/zh-cn/docs/writing.html)，写的很棒！然后再来细细讲讲文章（post）的模版文件，因为文章模版文件的个性化会直接影响文章（post）的主要布局，甚至是页面（page）的主要布局。

### 文章的模版文件（必读）

如果你是在站点文件夹根目录用`hexo new post <title>`新建的文章，那么其实它就是将文章的模版文件`post.md`「复制」了一份到`~/blog/source/_posts/`下，所以这也意味着：

1. 你可以直接通过在`~/blog/source/_posts/`下新建`.md`结尾的文件来写新的文章。
2. 你可以通过自定义文章的模版文件，从而每次命令行新建的文章都会有你自定义的内容。

注意：如果自己直接新建文件，一定要记得加上文件最上方的参数，即下面的相关内容，还有编码请用 UTF-8。

关于文件最上方的参数，参见 Hexo 官方文档的 [Front-matter](https://hexo.io/docs/front-matter.html) 和[页面变量](https://hexo.io/zh-cn/docs/variables.html#页面变量)，下面是我的总结：

```
/* ！！！！！！！！！！
** 每一项的 : 后面均有一个空格
** 且 : 为英文符号
** ！！！！！！！！！！
*/

title:
/* 文章标题，可以为中文 */

date:
/* 建立日期，如果自己手动添加，请按固定格式
** 就算不写，页面每篇文章顶部的发表于……也能显示
** 只要在主题配置文件中，配置了 created_at 就行
** 那为什么还要自己加上？
** 自定义文章发布的时间
*/

updated:
/* 更新日期，其它与上面的建立日期类似
** 不过在页面每篇文章顶部，是更新于……
** 在主题配置文件中，是 updated_at
*/

permalink:
/* 若站点配置文件下的 permalink 配置了 title
** 则可以替换文章 URL 里面的 title（文章标题）
*/

categories:
/* 分类，支持多级，比如：
- technology
- computer
- computer-aided-art
则为technology/computer/computer-aided-art
（不适用于 layout: page）
*/

tags:
/* 标签
** 多个可以这样写[标签1,标签2,标签3]
** （不适用于 layout: page）
*/

description:
/* 文章的描述，在每篇文章标题下方显示
** 并且作为网页的 description 元数据
** 如果不写，则自动取 <!-- more -->
** 之前的文字作为网页的 description 元数据
** 建议每篇文章都务必加上！
*/

keywords:
/* 关键字，并且作为网页的 keywords 元数据
** 如果不写，则自动取 tags 里的项
** 作为网页的 keywords 元数据
*/

comments:
/* 是否开启评论
** 默认值是 true
** 要关闭写 false
*/

layout:
/* 页面布局，默认值是 post，默认值可以在
** 站点配置文件中修改 default_layout
** 另：404 页面可能用到，将其值改为 false
*/

type:
/* categories，目录页面
** tags，标签页面
** picture，用来生成 group-pictures
** quote？
** https://reuixiy.github.io/uncategorized/2010/01/01/test.html
*/

photos:
/* Gallery support，用来支持画廊 / 相册，用法如下：
- photo_url_1
- photo_url_2
- photo_url_3
https://reuixiy.github.io/uncategorized/2010/01/01/test.html
*/

link:
/* 文章的外部链接
** https://reuixiy.github.io/uncategorized/2010/01/01/test.html
*/

image:
/* 自定义的文章摘要图片，只在页面展示，文章内消失
** 
*/

sticky:
/* 文章置顶
** 
*/

password:
/* 文章密码，此项只有参考教程：
** http://shenzekun.cn/hexo的next主题个性化配置教程.html
** 
*/
```

基于你自己博客的功能需求，以及自己对页面布局和文章布局的理解（审美观），将上面这些自己需要的加到自己的文章模版文件中即可，最后贴出我的文章模版文件：

```
文件位置：~/blog/scaffolds/post.md---
title: {{ title }}
date: {{ date }}
permalink:
categories:
tags: []
description:
image:
---
<p class="description"></p>

<img src="https://" alt="" style="width:100%" />

<!-- more -->

##

##

##

<hr />
```

但为什么说文章模版文件的个性化会直接影响文章（post）的主要布局，甚至是页面（page）的主要布局呢？哈哈哈，让我来解释一番我的文章模版文件，你就知道了～

- **Front-matter**

先起好文章的`title`（中文），然后补上`permalink`（英文），这是因为我在站点配置文件将 title 加进了文章的 URL 中，然而我不希望文章的 URL 中出现中文，相当于将 URL 中的中文翻译成英文。

`date`可以方便自定义文章的发布时间，接下来是[两种不同的分类方式](https://reuixiy.github.io/philosophy/how/2017/12/11/categories+tags.html)——`categories`和`tags`（分类和标签），`description`是文章的描述，建议务必写上！原因上面说了～

- **<p>**

由于文章的描述（`description`）有时候并不能满足我的需求有时候我需要将「文章的描述」写在文章中，并放在`<!-- more -->`之前作为文章摘要，但为了保证在文章（ post ）中与正文区分开来，我自己在`custom.styl`中自定义了一个`<p>`标签的样），用来模仿 description 的显示效果。

而且，由于如果不写`description`，则自动取`<!-- more -->`之前的文字作为网页的 description 元数据，外观和功能兼得，完美替代啊

- **图片**

由于`<!-- more -->`之前的文章内容会作为文章摘要（excerpt）显示在页面（page）中，会影响页面的主要布局，所以我在它前面加上了图片，为显示在页面的每篇文章的文章摘要配一张图片，保证博客页面的美观。有的图片宽度不够导致风格不够统一，页面不美观咋办？加上`width:100%`。

但是，有些情况下，也许你会从文章中选一张图片作为文章摘要图片，而这张图片由于写作要求，必须添加在文章的末尾，这样点 [Read More] 查看文章时，这张图片就会重复出现了，那怎么办？用[文章摘要图片 `image`

- **<!-- more -->**

之后的是文章的主要内容，文章的主要内容一般都可以分成几个部分，每一部分都可以概括出一个小标题，所以先将三个`##`（二级标题）加上咯～

- **hr**

最后加上`<hr />`作为分隔线，可以将文章内容与它下面的非文章内容分隔开来，

**既然自定义的<p>能替代description，那还有必要用description吗？**

### 使用 Markdown

用 Hexo 写文章是直接用 Markdown 写的，而不是像 WordPress 有一个类似 Word 一样的文字编辑器，所以第一次用会感觉有点难，但你熟练之后，就会觉得文字编辑器都是辣鸡

#### Markdown 简介

Markdown 的目标是实现「易读易写」。

不过最需要强调的便是它的可读性。一份使用 Markdown 格式撰写的文件应该可以直接以纯文字发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，然而最大灵感来源其实是纯文字的电子邮件格式。

因此 Markdown 的语法全由标点符号所组成，并经过严谨慎选，是为了让它们看起来就像所要表达的意思。像是在文字两旁加上星号，看起来就像**强调**。Markdown 的清单看起来，嗯，就是清单。假如你有使用过电子邮件，区块引言看起来就真的像是引用一段文字。

#### 总结

首先是基础但重要：

1. 文章内标题请从二级标题（`##`）开始！
2. 英语单词、数字左右看情况加上空格！
3. Markdwon 文档写完一段回车后务必再回车一次空一行！

然后如果有些用 Markdwon 的语法却达不到预期效果（甚至产生奇怪的 bugs），或者用 Markdwon 的语法无法实现，这时就可以考虑用 HTML 和 CSS。

- 分隔线和空行

```
/* 分隔线 */
<hr />
/* 注意事项[6]：在XHTML 中，<hr> 必须被正确地关闭，比如 <hr /> */

/* 空行 */
<br />
/* 注意事项同上 */
```

- 引用

```
<blockquote>引用内容</blockquote>

/* 如果上下间距很小，可以像下面这样写 */
<p><blockquote>引用内容</blockquote></p>
```

- 居中和右对齐

```
/* 居中 */
<center>内容</center>

/* 右对齐 */
<p style="text-align:right">内容</p>
```

- 字体大小和颜色

```
<font color="#xxxxxx" size="number">内容</font>
/* 详细请查看 http://www.w3school.com.cn/tags/tag_font.asp */
```

- Todo list

```
<ul>
<li><i class="fa fa-check-square"></i> 已完成</li>
<li><i class="fa fa-square"></i> 未完成</li>
</ul>
```



### 插入音乐和视频

音乐的话，网易云音乐的外链很好用，不仅有可以单曲，还能有歌单，有兴趣的自己去[网易云音乐](https://music.163.com/)找首歌尝试。但是有一些音乐因为版权原因放不了，还有就是不完全支持 https，导致小绿锁不见了。要解决这些缺点，就需要安装插件

#### 音乐

可以直接用 HTML 的标签，写法如下：

```
<audio src="https://什么什么什么.mp3" style="max-height :100%; max-width: 100%; display: block; margin-left: auto; margin-right: auto;" controls="controls" loop="loop" preload="meta">Your browser does not support the audio tag.</audio>
```

用插件，有显示歌词功能，也美观，建议使用这种方法。

首先在站点文件夹根目录安装插件：

```
所在目录：~/blog/npm install hexo-tag-aplayer --save
```

然后文章中的写法：

```
{% aplayer "歌曲名" "歌手名" "https://什么什么什么.mp3" "https://封面图.jpg" "lrc:https://歌词.lrc" %}
```

效果，在这博客的标签里自己找一篇文章看看。

另外可以支持歌单：

```
{% aplayerlist %}
{
    "autoplay": false,
    "showlrc": 3,
    "mutex": true,
    "music": [
        {
            "title": "歌曲名",
            "author": "歌手名",
            "url": "https://什么什么什么.mp3",
            "pic": "https://封面图.jpg",
            "lrc": "https://歌词.lrc"
        },
        {
            "title": "歌曲名",
            "author": "歌手名",
            "url": "https://什么什么什么.mp3",
            "pic": "https://封面图.jpg",
            "lrc": "https://歌词.lrc"
        }
    ]
}
{% endaplayerlist %}
```

#### 视频

可以直接用 HTML 的标签，写法如下：

```
<video poster="https://封面图.jpg" src="https://什么什么什么.mp4" style="max-height :100%; max-width: 100%; display: block; margin-left: auto; margin-right: auto;" controls="controls" loop="loop" preload="meta">Your browser does not support the video tag.</video>
```

用插件，功能更加强大，比如可以弹幕，非常建议食用。

首先在站点文件夹根目录安装插件：

```
所在目录：~/blog/npm install hexo-tag-dplayer --save
```

然后文章中的写法：

```
{% dplayer "url=https://什么什么什么.mp4" "https://封面图.jpg" "api=https://api.prprpr.me/dplayer/" "id=" "loop=false" %}
```

要使用弹幕，必须有`api`和`id`两项，并且若使用的是官方的 api 地址（即上面的），id 的值不能与[这个列表](https://api.prprpr.me/dplayer/list)的值一样。id 的值自己随便取，唯一要求就是前面这点。

#### 主题自带样式 代码块高亮

先看效果：

```
Java代码来自这/**
 * @author John Smith <john.smith@example.com>
*/
package l2f.gameserver.model;

public abstract class L2Char extends L2Object {
  public static final Short ERROR = 0x0001;

  public void moveTo(int x, int y, int z) {
    _ai = null;
    log("Should not be called");
    if (1 > 5) { // wtf!?
      return;
    }
  }
}
```

这里指的是`````代码块，而不是行内代码块（``代码``），它的用法如下：

\````[language] [title] [url] [link-text]`

```
代码
```

\`````

- [language] 是代码语言的名称，用来设置代码块颜色高亮，非必须；
- [title] 是顶部左边的说明，非必须；
- [url] 是顶部右边的超链接地址，非必须；
- [link text] 如它的字面意思，超链接的名称，非必须。

亲测这 4 项应该是根据空格来分隔，而不是`[]`，故请不要加`[]`。除非如果你想写后面两个，但不想写前面两个，那么就必须加`[]`了，要这样写：`[] [] [url] [link text]`。

首先关于代码块颜色高亮，高亮的模式可以在主题配置文件中设置：

```
# Code Highlight theme
# Available value:
#    normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme

highlight_theme: normal
```

要颜色正确高亮，代码语言的名称肯定要写对，各种支持语言的名称可以查看[这篇文章](https://almostover.ru/2016-07/hexo-highlight-code-styles/)。当然，如果你和我一样懒，可以在站点配置文件`_config.yml`中设置自动高亮：

```
文件位置：~/blog/_config.ymlhighlight:
  enable: true
  line_number: true
# 代码自动高亮
-  auto_detect: false
+  auto_detect: true
```

`diff`，所以你只需在 [language] 这写`diff`，然后在相应代码前面加上`-`和`+`就行了。不过默认的`-`是绿色，`+`是红色，与 GitHub 上相反，别扭就自己改成 GitHub 的咯：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 文章```代码块diff样式
pre .addition {
    background: #e6ffed;
}
pre .deletion {
    background: #ffeef0;
}
```

当然，要是你不满意顶部的文字样式，也可以自己在`custom.styl`自定义：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 文章```代码块顶部样式
.highlight figcaption {
    margin: 0em;
    padding: 0.5em;
    background: #eee;
    border-bottom: 1px solid #e9e9e9;
}
.highlight figcaption a {
    color: rgb(80, 115, 184);
}
```

参考了 Hexo 官方文档的[标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html#代码块)页面，这个页面还有更多的 Hexo 标签插件（Tag Plugins）的用法，请自行查看。

关于代码块高亮的高级个性化，请查看大佬的文章——[HEXO下的语法高亮拓展修改](https://www.ofind.cn/blog/HEXO/HEXO%E4%B8%8B%E7%9A%84%E8%AF%AD%E6%B3%95%E9%AB%98%E4%BA%AE%E6%8B%93%E5%B1%95%E4%BF%AE%E6%94%B9.html)。

#### 主题自带样式 文本居中引用

效果：

> 人生乃是一面镜子，
> 从镜子里认识自己，
> 我要称之为头等大事，
> 也只是我们追求的目的！

源码：

```
{% cq %}
人生乃是一面镜子，
从镜子里认识自己，
我要称之为头等大事，
也只是我们追求的目的！
{% endcq %}
```

更多 NexT 主题自带的标签样式，请点击：<http://theme-next.iissnan.com/tag-plugins.html>

#### 主题自带样式 note 标签

在主题配置文件`_config.yml`里有一个关于这个的配置，但官方文档没有提供 HTML 的使用方式，个人认为这种方式更简单，也不会产生一些奇怪的显示 bugs……

default

```
<div class="note default"><p>default</p></div>
```

primary

```
<div class="note primary"><p>primary</p></div>
```

success

```
<div class="note success"><p>success</p></div>
```

info

```
<div class="note info"><p>info</p></div>
```

warning

```
<div class="note warning"><p>warning</p></div>
```

danger

```
<div class="note danger"><p>danger</p></div>
```

danger no-icon

```
<div class="note danger no-icon"><p>danger no-icon</p></div>
```

首先可以在主题配置文件中需要配置下，贴上我的：

```
文件位置：~/blog/themes/next/_config.yml# Note tag (bs-callout).
note:
  # 风格
  style: flat
  # 要不要图标
  icons: true
  # 圆角矩形
  border_radius: 3
  light_bg_offset:
```

里面的三种风格长啥样？开启图标长啥样？可以查看[这个页面](https://github.com/iissnan/hexo-theme-next/pull/1697)，更多的介绍也在这个页面，请自行查看。

#### 主题自带样式 label 标签

首先可以在主题配置文件中有配置，需要配置下，贴上我的：

```
文件位置：~/blog/themes/next/_config.yml# Label tag.
label: true
```

然后效果如下（@ 前面的是`label`的名字，后面的是要显示的文字）：

- default

```
{% label default@default %}
```

- primary

```
{% label primary@primary %}

```

- success

```
{% label success@success %}

```

- info

```
{% label info@info %}

```

- warning

```
{% label warning@warning %}

```

- danger

```
{% label danger@danger %}

```

目前博主发现个 bug，如果把它加在一段文字的段首，则会有点问题，[issue](https://github.com/iissnan/hexo-theme-next/issues/2022) 页面。

#### 主题自带样式 tabs 标签

效果：

- [选项卡 1](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#选项卡-1)
- [选项卡 2](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#选项卡-2)
- [选项卡 3](https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#选项卡-3)

**这是选项卡 2**

源码：

```
{% tabs 选项卡, 2 %}
<!-- tab -->
**这是选项卡 1** 呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈呵呵哈哈哈哈哈哈哈哈……
<!-- endtab -->
<!-- tab -->
**这是选项卡 2**
<!-- endtab -->
<!-- tab -->
**这是选项卡 3** 哇，你找到我了！φ(≧ω≦*)♪～
<!-- endtab -->
{% endtabs %}

```

首先可以在主题配置文件中有配置，需要配置下，贴上我的：

```
文件位置：~/blog/themes/next/_config.yml# Tabs tag.
tabs:
  enable: true
  transition:
    tabs: true
    labels: true
  border_radius: 0

```

然后上面源码中`, 2`表示一开始在第二个选项卡，非必须，若数值为`-1`则隐藏选项卡内容。更多用法请查看[这个页面](https://almostover.ru/2016-01/hexo-theme-next-test/#Tab-tag-test)。

#### 主题自带样式 按钮

源码：

```
{% btn https://www.baidu.com, 点击下载百度, download fa-lg fa-fw %}

```

效果：[点击下载百度](https://www.baidu.com/)

关于按钮的更多使用可以前往[这个页面](https://almostover.ru/2016-01/hexo-theme-next-test/#Button-tag-test)查看。

#### 自定义样式 引用

首先由于是自定义的样式，故要自己将 CSS 代码加到`custom.styl`中，下文的自定义样式都是如此。为什么可以自定义呢？如果你是一个和我一样的小白，可以[点击这里](http://www.divcss5.com/rumen/r3.shtml)了解了解 CSS 中`id`和`class`的知识。

需加入`custom.styl`的代码：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的引用样式
blockquote.question {
    color: #555;
    border-left: 4px solid rgb(16, 152, 173);
    background-color: rgb(227, 242, 253);
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    margin-bottom: 20px;
}

```

- 文字颜色改`color`的值
- 背景色改`background-color`的值
- 边框颜色和粗细改`border-left`的值

效果：

> 内容

```
<blockquote class="question">内容</blockquote>

```

#### 自定义样式 数字块

需加入`custom.styl`的代码：

```
文件位置：~/blog/themes/next/source/css/_custom/custom.styl// 自定义的数字块
span#inline-toc {
    display: inline-block;
    border-radius: 80% 100% 90% 20%;
    background-color: rgb(227, 242, 253);
    color: #555;
    padding: 0.05em 0.4em;
    margin: 2px 5px 2px 0px;
    line-height: 1.5;
}

```

1.左边是效果。

源码：

```
<span id="inline-toc">1.</span>

```

参考：<https://qianling.pw/style/#TOC数字块>

## 转载

------

1. https://xian6ge.cn/posts/5b8c41e7/

2. https://reuixiy.github.io/technology/computer/computer-aided-art/2017/06/09/hexo-next-optimization.html#附上我的-custom-styl

3. https://www.bluelzy.com/articles/change_to_next_theme.html

   