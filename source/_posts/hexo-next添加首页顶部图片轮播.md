---
title: hexo-next添加首页顶部图片轮播
date: 2019-03-29 19:57:42
tags: 
  - hexo
  - 教程
categories: 教程
copyright:
---

## 加入局部代码

**在需要加入轮播图的页面部位加入代码**

比如我要在首页加轮播图，那么打开`themes\next\layout下`的`index`文件，找到：

```
{% block content %}
```

在这段代码的下面写上

```
{% include '_macro/carousel.swig' %}
```





## 配置主题config文件

在主题的`_congif.yml`文件中添加一段配置

```
#Home carousel map, from means link, img means picture
carousel: 
   enable: true
   item: [
      {
        'link':'文章链接1',
        'img':'图片链接1'
      },
      {
        'link':'文章链接2',
        'img':'图片链接2'
      },
   ]
```

几张轮播图就添加几个数组内容
**个人示例**

```
#Home carousel map, from means link, img means picture
carousel: 
   enable: true
   item: [
      {
        #'link':' ',
        'img':'http://wx4.sinaimg.cn/mw690/00639ahCly1g1jtuwnwluj30u00bowie.jpg'
      },
      {
        #'link':' ',
        'img':'http://wx4.sinaimg.cn/mw690/00639ahCly1g1jtwnr6bfj30u00boae5.jpg'
      },
      {
        #'link':' ',
        'img':'http://wx4.sinaimg.cn/mw690/00639ahCly1g1jtuti0xaj30u00bo0wa.jpg'
      },
   ]
```

## 创建carousel的swig文件

在`themes\next\layout\macro`中创建`carousel.swig`文件

添加以下内容

```
{% if theme.carousel.enable %}
<script src="https://cdn.staticfile.org/jquery/2.1.1/jquery.min.js"></script>
<script src="https://cdn.staticfile.org/twitter-bootstrap/3.3.7/js/bootstrap.min.js"></script>

<style type="text/css">

.glyphicon-chevron-left:before{
	content: "《"
}
.glyphicon-chevron-right:before{
	content: "》"
}

@media (max-width: 767px){
	.rights{
		display: none;
	}
	.carousel{
		width: 100% !important;
		height: 100% !important;
	}
	.slide{
		width: 100% !important;
		height: 100% !important;
	}

}

.carousel{
	width: 100%;
	height: 100%;
	position: relative;
}

.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
</style>
<div width="100%" height="320px" style="border: 0px; overflow: hidden; border-radius: 10px;" scrolling="no">
			
	<div id="myCarousel" class="carousel slide" data-ride="carousel" data-interval="3500" >
		<!-- 轮播（Carousel）指标 -->
		<ol class="carousel-indicators">
		{% set index = 0 %}
		{% for item in theme.carousel.item %}

			<li data-target="#myCarousel" data-slide-to="{{index}}"></li>
			{% set index = index+1 %}

		{% endfor %}
		</ol>
		<!-- 轮播（Carousel）项目 -->
		<div class="carousel-inner" style="height: 280px; border-radius: 10px; width: 100%;">

		{% set act = 0 %}
		 {% for item in theme.carousel.item %}
		
	    	{% if act===0 %}
				<a class="item active" href="{{ url_for(item.link) }}" target="_blank" style="height: 100%;">
					<img src="{{item.img}}"   style="width: 100%; height: 100%" >
				</a>
				{% set act = 1 %}
				{% elseif act===1 %}
					<a class="item" href="{{ url_for(item.link) }}" target="_blank" style="height: 100%;">
						<img src="{{item.img}}"  style="width: 100%; height: 100%;" >
					</a>
			{% endif %}

		{% endfor %}

	 
		</div>
		<!-- 轮播（Carousel）导航 -->
		<a class="left carousel-control" href="#myCarousel" role="button" data-slide="prev">
		    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
		    
		</a>
		<a class="right carousel-control" href="#myCarousel" role="button" data-slide="next">
		    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
		   
		</a>
	</div> 

</div>

{% endif %}
```

重新生成渲染

```
$ hexo clean
$ hexo g 
$ hexo d
```