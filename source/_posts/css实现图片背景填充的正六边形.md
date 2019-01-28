---
title: css实现图片背景填充的正六边形
date: 2019-01-28 11:09:42
tags: 'css'
categories: 'css技巧'
---

最近有个需求需要实现带有图片背景的正六边形，这里记录一下我自己实现的方法。

我项目里的最终效果大概是这样子的，这里我们只演示实现一个正六边形。

![](https://github.com/sundaypig/blog/raw/master/images/hex0.png)

<!-- more -->

六边形的实现原理其实就是通过旋转三个重叠的矩形得到的，如下图所示：

![](https://github.com/sundaypig/blog/raw/master/images/hex1.png)

这里为了得到一个正的六边形，两个矩形旋转的角度必须为-60deg 和 60deg，以及矩形高宽比必须是 `Math.sqrt(3) : 1`， 原谅我不会打根号 3 _(:3」∠)_。

那么首先我们得创建三个重叠的矩形：

```html
<div class="hexagon">
    <div class="hexagon__item hexagon__item_left"></div>
    <div class="hexagon__item hexagon__item_center"></div>
    <div class="hexagon__item hexagon__item_right"></div>
</div>
```

我们设定三个矩形的宽高分别为 60px 和 104px，背景色为蓝色，`.hexagon__item_left` 旋转-60deg，`.hexagon__item_right` 旋转 60deg，`.hexagon__item_center` 不旋转。

```css
.hexagon {
    width: 60px;
    height: 104px;
    position: relative;
    margin: 200px auto;
}

.hexagon__item {
    width: 100%;
    height: 100%;
    background: blue;
    position: absolute;
    top: 0;
    left: 0;
}

.hexagon__item_left {
    transform: rotate(-60deg);
}

.hexagon__item_right {
    transform: rotate(60deg);
}
```

这样就很简单得到了下面这个六边形了。

![](https://github.com/sundaypig/blog/raw/master/images/hex3.png)

那么我们要如何才能使得蓝色背景变成图片呢，其实也很简单，上述的三个矩形其实只是起到了一个塑形的作用，实际上是应该设置为 `visibility: hidden` 的，我们需要给三个矩形分别添加一个矩形的子元素并且设置为 `visibility: visible` 。三个子元素的宽高需要正好能覆盖之前的蓝色六边形，如下图示意：

![](https://github.com/sundaypig/blog/raw/master/images/hex2.png)

之后给上面橘色的矩形设置背景图片，给它的父元素也就是.hexagon\_\_item 设置`overflow: hidden` 就差不多大功告成了。

为了使得 html 结构精简一些，这里使用伪元素作为矩形的子元素。

```css
.hexagon {
    width: 60px;
    height: 104px;
    position: relative;
    margin: 200px auto;
}

.hexagon__item {
    width: 100%;
    height: 100%;
    background: blue;
    position: absolute;
    top: 0;
    left: 0;
    visibility: hidden;
    overflow: hidden;
}

.hexagon__item_left {
    transform: rotate(-60deg);
}

.hexagon__item_right {
    transform: rotate(60deg);
}

.hexagon__item:before {
    position: absolute;
    top: 0;
    left: 0;
    content: '';
    height: 100%;
    width: 120px;
    visibility: visible;
    background: url('https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=13897784,1115290966&fm=58') no-repeat;
    background-size: cover;
    transform-origin: 0 0;
}
```

上面的伪元素宽度为 120px 是怎么计算得来的呢？其实正好就是其父元素宽度的 2 倍，不熟悉勾股定理的赶紧去复习一下就好了，哈哈！  
于是我们得到了下面的样子。

![](https://github.com/sundaypig/blog/raw/master/images/hex4.png)

看起来效果还差了点，最后一步我们只需要把几个伪元素旋转变换一下即可。这里注意一下，伪元素的旋转中心需要设置为 `transform-origin: 0 0` ，即以左上角为中心旋转。

```css
.hexagon__item_left:before {
    transform: rotate(60deg) translateY(-50%);
}

.hexagon__item_right:before {
    transform: rotate(-60deg) translateX(-75%);
}

.hexagon__item_center:before {
    transform: translateX(-25%);
}
```

什么， 你问我为什么伪元素的旋转变换又是 rotate 60deg，又是 translate 50% 20 % 75% 的？

我当然也是自己慢慢试出来的啊，我数学也不好哈，sorry！  
最后我们的图片背景的正六边形就完成了。

![](https://github.com/sundaypig/blog/raw/master/images/hex5.png)

最后的最后，奉上完整代码，需要的同学尽管拿去研究吧，哈！

```html css
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
        <style>
            .hexagon {
                width: 60px;
                height: 104px;
                position: relative;
                margin: 200px auto;
            }

            .hexagon__item {
                width: 100%;
                height: 100%;
                background: blue;
                position: absolute;
                top: 0;
                left: 0;
                visibility: hidden;
                overflow: hidden;
            }

            .hexagon__item_left {
                transform: rotate(-60deg);
            }

            .hexagon__item_right {
                transform: rotate(60deg);
            }

            .hexagon__item:before {
                position: absolute;
                top: 0;
                left: 0;
                content: '';
                height: 100%;
                width: 120px;
                visibility: visible;
                background: url('https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=13897784,1115290966&fm=58') no-repeat;
                background-size: cover;
                transform-origin: 0 0;
            }

            .hexagon__item_left:before {
                transform: rotate(60deg) translateY(-50%);
            }

            .hexagon__item_right:before {
                transform: rotate(-60deg) translateX(-75%);
            }

            .hexagon__item_center:before {
                transform: translateX(-25%);
            }
        </style>
    </head>

    <body>
        <div class="hexagon">
            <div class="hexagon__item hexagon__item_left"></div>
            <div class="hexagon__item hexagon__item_center"></div>
            <div class="hexagon__item hexagon__item_right"></div>
        </div>
    </body>
</html>
```
