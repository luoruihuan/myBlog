---
title: 原生js实现幻灯片轮播效果
date: 2016-2-18 20:00:00
categories: 前端
tags: js轮播
---


在以往的认知中，一直以为用原生JS写轮播是件很难得事情，今天上班仿照网上的写了一个小demo。小试牛刀。

大致效果：

![swiper-bg](uploads/swiper-bg.png)

html结构很简单，两个列表，一个代表图片列表，一个是右下角序号列表。

<!-- more -->

```html
    <div id="box">
        <ul class="list">
            <li><img src="01.jpg" width="490" height="170" /></li>
            <li><img src="02.jpg" width="490" height="170" /></li>
            <li><img src="03.jpg" width="490" height="170" /></li>
            <li><img src="04.jpg" width="490" height="170" /></li>
            <li><img src="05.jpg" width="490" height="170" /></li>
        </ul>
        <ul class="count">
            <li class="current">1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
        </ul>
    </div>
```
布局的大致想法是图片全部绝对定位重合，透明度为1，右下角序号给选中的添加样式。样式都很简单。

关于JS方面，自己还是捉摸了很久，刚开始实现时，当鼠标移入右下角对应序列号时图片能够准时切换，但是鼠标移出时便不能自动播放，关于setInterval和clearInterval自己的处理还是不是很好。借鉴了网上的案列之后，具体的实现逻辑简单为，先写一个图片的展现方法，和鼠标移出图片时的定时函数。这样可以在事件中直接调用，值得注意的一点就是在鼠标移入时，记得清除定时器。下面上代码：

这是图片展示函数，参数为当前的展示的index。先让所有图片透明度为0；所有下标没有样式，然后给当前的加上样式，透明度为1；
```js
    function show(a){
        for(var i=0;i<oLi.length;i++){
            oLi[i].style.opacity= 0;
            oImg[i].className = '';
        }
        oLi[a].style.opacity = 200;
        oImg[a].className = 'current';
    }
```

然后写一个鼠标移除时的定时器，图片自动轮播函数：

```js
    function autoplay(){
        time = setInterval(function(){
            b++;
            if(b>=oImg.length){
                b=0;
            }
            show(b);
        },1000);
    }
    autoplay();//自动运行
```

最后是鼠标移入移出事件:

```js
    for(var i=0;i<oImg.length;i++){
        oImg[i].index = i;
        oImg[i].onmouseover=function(){
            clearInterval(time);
            show(this.index)
        }
        oImg[i].onmouseout=function(){
            autoplay();
        }
    }
```

代码只是写了一个结构骨架，实现大致效果，具体可以优化的地方还有很多，对于动效方面可以对透明度的改变设置一个函数，让透明度缓慢变成1，或者对图片的定位和移动方向进行改变，实现真正意义上的左右轮播。大致原理都是差不多的。

原生JS实现轮播，于我而言还是有点小难度的，因为JS写的比较少，所以对于整体的逻辑把握不好，虽有曲折，但是靠着自己也是写了出来。

总结一些轮播遇到的大致困难，第一个是对于鼠标移出时的定时自动轮播，刚开始自己是写在事件onmouseout里面的，但是测试过程中发现事件只会触发一次，所以导致轮播不能执行，所以选择在重新写了一个方法。第二个就是对于函数的传参问题，我写函数一直没有传参的习惯，都是匿名函数，虽然知道适用场景不同，对函数的运用应该灵活，可能是因为做的项目太少，经验还是太不足，第三个就是细节方面，例如移入时清除定时器，轮播要先执行一次等。总之收获还是很大的。

感谢阅读。
