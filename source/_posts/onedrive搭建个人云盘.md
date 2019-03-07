---
title: onedrive搭建个人云盘
tags: oneIndex
categories: 教程
abbrlink: dbac08c7
date: 2019-03-03 14:28:13
copyright:

---

## OneDrive+OneIndex

OneIndex针对Onedrive网盘的一个开源程序。可以将Onedrive存储的文件展示，直连下载。**视频还能在线播放**！不用服务器空间，不走服务器流量！

### 功能：

- 不用服务器空间，不走服务器流量，
- 直接列OneDrive目录，文件直链下载。
- HTML以及markdown图床
- 视频在线播放

### 准备工作

1）一台支持PHP的虚机 或 VPS 或 Docker等

2）准备一个OneDrive账号。

3）(可选)准备一个域名用于解析。

### 开源项目

OneIndex2.0是hostloc论坛大佬@[donwa](http://www.hostloc.com/space-uid-18200.html) 写出来的PHP程序，利用OneDrive的API接口，程序可以直接列出你的OneDrive目录，和普通的Index列表程序一样简单。它的优点？

1. 响应式，支持小屏设备
2. 图片在线预览
3. 视频、音频在线播放
4. 代码在线查看（js、css、html、sh、php、java、md等）
5. README.md 支持，解析各目录下(onedirive目录下) README.md 文件，在页面尾部展示。HEAD.md 支持，在页面头部展示。
6. PHP程序安装使用简单，仅需PHP5.6+ 加Curl支持即可，甚至一个几十块钱一年的虚拟主机就行，无需任何数据库支持。
7. 不消耗本机流量，完全无需担心流量超支。
8. 变相支持了直链功能，你甚至可以当视频床、图床用，可以方便分享文件给你的朋友，当然不能太玩过火，官方可能会封流量过大的用户。

<https://github.com/donwa/oneindex>

### 平台设置

上传到主机目录即可，请务必给config/ 、config/base.php 、 cache/读写权限，一般为755或777。

已经内置了nginx、apache主流环境的伪静态规则，直接使用即可。

输入你的网址打开，直接会提示授权，登录你的OneDrive帐号，授权API即可。非常简单！

1. #### 平台设置

1）访问部署的网址，第一步检查软件环境。

PHP > 5.5

CURL支持

congfig 和 cache目录可读写

![](http://wx4.sinaimg.cn/mw690/00639ahCgy1g0ppu7swxij30i908oaae.jpg)

2）获取应用ID 和 密钥。点击自动跳转！

![](http://wx2.sinaimg.cn/mw690/00639ahCgy1g0ppuc6yf5j30iq0adq3g.jpg)

3）登陆你的OFFICE账号！

![](http://wx1.sinaimg.cn/mw690/00639ahCgy1g0ppuh9134j30h20efjrp.jpg)

4）第一个显示的是密钥，复制到设置中。然后点击【知道了，返回到快速启动】

![](http://wx2.sinaimg.cn/mw690/00639ahCgy1g0pqunjaa4j30ib082wf5.jpg)

5）下面显示的是应用ID，复制到设置中。

![](http://wx2.sinaimg.cn/mw690/00639ahCgy1g0pqus2n6rj30ib08emxm.jpg)

6）将以上的内容全部粘贴到下面。点击【下一步】

![](http://wx2.sinaimg.cn/mw690/00639ahCgy1g0pquwnxy9j30il0bcmxn.jpg)

7）点击【账号绑定】来绑定我们的OFFICE账号！

![](http://wx4.sinaimg.cn/mw690/00639ahCgy1g0pqv0cdmej30hz059dft.jpg)

8）点击接受，即可完成绑定！

![](http://wx4.sinaimg.cn/mw690/00639ahCgy1g0pqv4vsldj30e90cyt9b.jpg)

![](http://wx2.sinaimg.cn/mw690/00639ahCgy1g0pqw91fdlj30cn0duaax.jpg)

9）点击【管理后台】可以将默认密码修改一下，然后是一些设置。

![](http://wx3.sinaimg.cn/mw690/00639ahCgy1g0pqwdtjlcj30i106i0su.jpg)



![](http://wx4.sinaimg.cn/mw690/00639ahCgy1g0pqwivxs3j30hw08baa4.jpg)





![](http://wx1.sinaimg.cn/mw690/00639ahCgy1g0pqwnndl4j30iz0810sv.jpg)



## 计划任务

[可选]**推荐配置**，非必需。后台定时刷新缓存，可增加前台访问的速度

```
# 每小时刷新一次token
0 * * * * /具体路径/php /程序具体路径/one.php token:refresh

# 每十分钟后台刷新一遍缓存
*/10 * * * * /具体路径/php /程序具体路径/one.php cache:refresh
```

## 特殊文件实现功能

`README.md`、`HEAD.md` 、 `.password`特殊文件使用

可以参考<https://github.com/donwa/oneindex/tree/files>

**在文件夹底部添加说明:**

> 在onedrive的文件夹中添加`README.md`文件，使用markdown语法。

**在文件夹头部添加说明:**

> 在onedrive的文件夹中添加`HEAD.md` 文件，使用markdown语法。

**加密文件夹:**

> 在onedrive的文件夹中添加`.password`文件，填入密码，密码不能为空。

## 命令行功能

仅能在php cli模式下运行
 **清除缓存:**

```
php one.php cache:clear
```

**刷新缓存:**

```
php one.php cache:refresh
```

**刷新令牌:**

```
php one.php token:refresh
```

**上传文件:**

```
php one.php upload:file 本地文件 [onedrive文件]
```

例如：

```
//上传demo.zip 到onedrive 根目录  
php one.php upload:file demo.zip  

//上传demo.zip 到onedrive /test/目录  
php one.php upload:file demo.zip /test/  

//上传demo.zip 到onedrive /test/目录并命名为 d.zip
php one.php upload:file demo.zip /test/d.zip  
```

## 可配置项

配置在 `config/base.php` 文件中:

**onedrive共享的起始目录:**

```
'onedrive_root'=> '', //默认为根目录
```

如果想只共享onedrive下的 /document/share/ 目录

```
'onedrive_root'=> '/document/share', //最后不带 '/'
```

**去掉链接中的 /?/ :**
 需要添加apache/nginx/iis的rewrite的配置文件
 参考程序根目录下的：`.htaccess`或`nginx.conf`或`Web.config`

```
  //在config/base.php 中
  'root_path' => '?' 
```

改为

```
    'root_path' => '' 
```

> nginx图片404问题,参考<https://github.com/donwa/oneindex/issues/14>

 

设置nginx伪静态

```
location / {
    if (!-f $request_filename){ 
        rewrite (.*) /index.php; 
    } 
} 
```

 

**缓存时间:**   初步测试直链过期时间为一小时,默认设置为：

```
  'cache_expire_time' => 3600, //缓存过期时间 /秒
  'cache_refresh_time' => 600, //缓存刷新时间 /秒
```

如果经常出现链接失效，可尝试缩短缓存时间,如:

```
  'cache_expire_time' => 300, //缓存过期时间 /秒
  'cache_refresh_time' => 60, //缓存刷新时间 /秒
```