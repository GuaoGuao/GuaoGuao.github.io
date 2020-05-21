---
title: css动画
date: 2020-05-14 11:39:10
tags:
---

# 概述

css中动画效果一般都是咋实现的呢，参考[阮一峰大神的博客](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)和[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)学习一下。

主流用的就是两种，transition 和 animation

# transition

* 为一个元素在不同状态之间切换的时候定义不同的过渡效果。可以说只要dom变化就会通过指定动画变化
  * 在不同的伪元素之间切换，像是 :hover，:active。
  * 通过 JavaScript 实现的变化。
* 原理是指定一个开始状态和结束状态，计算补间动画
* 可以指定一个或多个动画，多个用逗号分隔
```JavaScript
img{
  transition: 1s height, 1s width;
}
```
* 包含了transition-property，transition-duration，transition-timing-function 和 transition-delay
  * transition-property，动画属性，all表示全部动画，none表示都不动画，指定具体的属性可以参考[这个](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)，常用的大部分都有，另外[常见效果参考
  ](http://leaverou.github.io/animatable/#)
  * transition-duration，以s或ms为单位指定过渡动画所需的时间，默认值为 0s
  * transition-timing-function，指定动画变化方式，常用的如下，[其他参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function)
      ```
    （1）linear：匀速
    （2）ease-in：加速
    （3）ease-out：减速
    （4）cubic-bezier函数：自定义速度模式
      ```
  * transition-delay，以秒（s）或毫秒（ms）为单位，动画延时时间
* transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。
* 想让页面加载结束就开始动画，可以在window.ready中修改css样式实现
* transition/translate/transform/rotate
  * transform属性允许你旋转，缩放，倾斜或平移给定元素。这是通过修改CSS视觉格式化模型的坐标空间来实现的。
  * translate 和 rotate 是 transform 的属性，表示dom变换方式

# animation

* 包含了animation-name，animation-duration, animation-timing-function，animation-delay，animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 
  * animation-name: 指定动画名，然后通过 @keyframes 定义动画
  * animation-duration: 动画时间，s或ms为单位
  * animation-timing-function: 同transition-timing-function
  * animagion-delay: 同 transition-delay
  * animation-iteration-count: 循环次数，整数、小数或者infinite
  * animation-direction: normal/alternate/reverse/alternate-reverse，即正向、往返、反向、反向往返
  * animation-fill-mode: none/forwards/backwards/both，定义动画结束后的状态
  * animate-play-state: paused/running， 表示动画暂停、运行

  ```JavaScript
  // MDN上的例子
  .cylon_eye {
    background-color: red;
    background-image: linear-gradient(to right,
        rgba(0, 0, 0, .9) 25%,
        rgba(0, 0, 0, .1) 50%,
        rgba(0, 0, 0, .9) 75%);
    color: white;
    height: 100%;
    width: 20%;

    -webkit-animation: 4s linear 0s infinite alternate move_eye;
            animation: 4s linear 0s infinite alternate move_eye;
  }

  @-webkit-keyframes move_eye { from { margin-left: -20%; } to { margin-left: 100%; }  }
          @keyframes move_eye { from { margin-left: -20%; } to { margin-left: 100%; }  }
  ```
* 使用 @keyframes 定义动画序列

  关键帧中重复定义的，以最后一次为准，出现！important的都将被忽略
  ```JavaScript
  @keyframes identifier {
    0% { top: 0; left: 0; }
    30% { top: 50px; }
    68%, 72% { left: 50px; }
    100% { top: 100px; left: 100%; }
  }
  ```
* 性能方面，都是动画实现一个div高度伸长，通过chrome的Performace分析，这是用定时器循环修改的
  ![](/public/images/animate1.png)
  这是通过transition
  ![](/public/images/animate2.png)
  这是用了animation
  ![](/public/images/animate3.png)
  用定时器因为要运行JavaScript所以性能最差，另外两种animation所需的rendering时间比transition少一些
  动画每帧需要做这些事
  * 获取DOM后分割为多个图层
  * 对每个图层的节点计算样式结果（Recalculate style--样式重计算）
  * 为每个节点生成图形和位置（Layout--回流和重布局）
  * 将每个节点绘制填充到图层位图中（Paint Setup和Paint--重绘）
  * 图层作为纹理上传至GPU
  * 符合多个图层到页面上生成最终屏幕图像（Composite Layers--图层重组）
  不知道为啥像是transition的回流和重绘时间比animation时间长一些

# jQuery的animate方法

* 使用方法
```JavaScript
$(selector).animate(styles,speed,easing,callback)
// 举个栗子
$("#box").animate({height:"300px"});
```

* 原理是通过 tweenjs 创建补间动画，css不断的修改实现的动画

![](/public/images/animate4.png)

# animate Css库

* 见[官网介绍](https://daneden.github.io/animate.css/)
* 其实就是用 animation 做的一堆效果封装成一些class，用的时候可以直接引，感觉直接copy出来用也行
