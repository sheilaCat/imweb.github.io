title: animation动画实践
date: 2015-08-03 16:35:18
tags: CSS/重构
---
由于业务关系，有幸参与腾讯课堂app下载页面制作，原则上ie8、9可表现为静态版本，而高级浏览则为动画版本。在这把一些重要思想和中间遇到的一些问题记录下，以供知识积累及讨论交流。
区分浏览器
既然要实现高级浏览器与低级浏览器不同效果，所以必要的区分下浏览器，使用js给body添加class
var lte9 = document.all && !window.atob;

if(lte9){
    //$('body').addClass('lte9');
    document.documentElement.className += ' lte9'
}else{
    //$('body').addClass('modern');
    document.documentElement.className += ' modern'
}
全屏元素布局
背景图片（这个是不兼容的，可以通过js来解决，这里因为图片本身很大，所以直接忽略不兼容，设置background-position: center，那样即使有超大的屏幕，也可以实现居中显示）：
background-size: cover;
由于是全屏设计，所以可能会遇到屏大屏小，元素参考点以中间为标准，向上下左右延展：
position: absoulte;
top: 50%;
left: 50%;
margin-left: npx;
margin-top: npx;
或者嵌套一层div，设置
display:table-cell;
vertical-align:midddle;
元素默认位置原则：
先按照psd稿设置所处位置，那样就不需要给lte9进行位置重置，高级浏览器再通过关键帧动画改变起点位置，注意动画中位置偏移应使用translate而不是top/left或margin值
进入动画
第一种是每次进入都有动画，第二种方案是只有第一次进入有动画，对于之后的滚动都是静态模式
如果采用第一种，我们把动画控制的class绑定在js切换的active上即可
如果采用第二种，就需要另起一个class，如这里用的on
动画元素一个个出现
因为动画元素得按照顺序一个个出现，所以在运动之前视觉上是看不见的。这里有两种方法处理：
默认设置动画元素的opacity为0，再给lte9的重置为1
默认不处理，给modern的设置opacity为0，这里采用第二种，给高级浏览器动画元素设置opacity为0
缩放动画
默认缩放动画统一采用scale，但是特殊情况除外
如果要保持某些像素不虚则不适合使用scale，因为在扩大的时候就会变虚，如波纹，如果通过border或box-shadow扩展出来的都比较粗且虚，所以使用width和height改变大小，通过translate改变位移来实现扩展动画
如果使用scale缩放背景图片也虚得一塌糊涂，所以放弃背景图片而采用img标签，设置img的width:100%;，scale父元素的时候，img也会响应的扩展，且不影响画质，第一屏的两层切换就是用了在scale的元素中使用了img，而非直接背景图
多个相当元素依次进入动画
通过设置animation-delay来依次进入动画，如流星，波纹圆圈
.meteor-list .meteor-item｛
    animation: meteoFlush 2.4s 0.12s linear infinite;
｝
.meteor-list .item--2{
    animation-delay: 1.3s;
}
.meteor-list .item--3{
    animation-delay: 0.8s;
}
infinite动画中间有空档
如流星划过，动画应该是持续的，中间可能隔段时间又重新开始，如动画时间为1.2s，而间隔时间为1.2s。就可以通过设置动画时间为2.4s，而关键帧的设置可以在50%的时候就到达运动结束的位置，也就是50%-100%这段时间其实就是空出来的间隔时间。
.meteor-list .meteor-item｛
    animation: meteoFlush 2.4s 0.12s linear infinite;
｝

[[@-webkit-keyframes](/user/-webkit-keyframes)](/user/-webkit-keyframes) meteoFlush{
    0%{
        opacity: 0;
        transform: translate3d(55px, -35px, 0);
    }
    25%{
        opacity: 1;
    }
    50%, 100%{
        opacity: 0;
        transform: translate3d(-85px, 35px, 0);
    }
}
动画暂停
当进入第一屏的第二层时，流星动画暂停
.s-1-2-on .meteor-item {
  animation-play-state: paused;
}
多次动画
如“学习成就梦想”实现了三次动画，刚进入的时候是fade in动画，滚动进入第二层的时候是缩小动画，往回滚是放大动画
抓住最终结束状态，并设置为默认的css，这里最终结束状态有两个，一个是第一层的时候大小为原始大小，一个是第二层的时候大小为一半大小。
.s-1 .s-slogan {
  position: absolute;
  width: 588px;
  height: 93px;
  top: 50%;
  left: 50%;
  margin-left: -294px;
  margin-top: -230px;
  z-index: 20;
  opacity: 1;
}

.s-1-2-on .s-slogan {
  transform: scale(.5);
  transform-origin: center bottom;
  opacity: 1;
}
动画的实现通过class来控制，如刚进入的fade in动画，父元素追加classs-1-1-on
.s-1-1-on .s-slogan {
  animation: fadeIn 1.3s 1.1s cubic-bezier(.67,.01,.38,1) forwards;
}
进入第二层的时候，父元素追加classs-1-2-on in，离开的时候先把in换成out，再动画结束之后删除追加的s-1-2-on out
.s-1-2-on.in .s-slogan{
    animation: ttShrink 1s forwards;
}
.s-1-2-on.out .s-slogan{
    animation: ttShrink 1s reverse forwards;
}

[[[[@keyframes](/user/keyframes)](/user/keyframes)](/user/keyframes)](/user/keyframes) ttShrink{
    0%{
        transform: scale(1);
    }
    100%{
        transform: scale(.5);
    }
}
反向动画
见上面的in和out，注意正向和反向动画得把动画分别绑定在两个不同的class，而默认的class只负责设置为正向动画结束后停留的位置。
延迟动画
延迟的动画如果第一帧的透明度不是从0开始，得重新添加一个关键帧，不然会出现一个半透明的在等着动画。如：
.s-2.on .s-img-msg{
    animation: msgShow 0.2s 1s both;    
}
[[[[@keyframes](/user/keyframes)](/user/keyframes)](/user/keyframes)](/user/keyframes) msgShow{
    0%{
        opacity: 0;
        transform: scale(0.5);
    }
    1%{
        opacity: 0.5;
        -webkit-transform: scale(0.5);
    }
    100%{
        opacity: 1;
        transform: scale(1);
    }
}
因为opacity从0.5开始，而动画是延迟在1s后执行，所以如果不特殊处理，就意味着在opacity:0.5;的时候，会持续1s等待。这里将0%设置为opacity:0;，而把实际关键帧0.5放在了1%
多个动画结合于同一元素
第三屏对话框的动画，fade in和width动画结合
.on .chat-item--1 .item-text{
    animation-name: fadeIn, sizeExpanded1;
}
[[[[@keyframes](/user/keyframes)](/user/keyframes)](/user/keyframes)](/user/keyframes) fadeIn{
    0%{
        opacity: 0;
    }
    100%{
        opacity: 1;
    }
}
[[@-webkit-keyframes](/user/-webkit-keyframes)](/user/-webkit-keyframes) sizeExpanded1 {
    0% {
        height: 0;
        width: 0;
    }
    100% {
        height: 85px;
        width: 408px;
    }
}
多帧动画，通过多帧控制实现弹簧效果
可通过关键帧来控制
[[[[@keyframes](/user/keyframes)](/user/keyframes)](/user/keyframes)](/user/keyframes) qrFlip{
    0%{
        opacity: 0;
        transform: rotateX(50deg);
    }
    70%{
        transform: rotateX(-10deg);
    }
    80%{
        transform: rotateX(6deg);
    }
    90%{
        transform: rotateX(-4deg);
    }
    95%{
        transform: rotateX(2deg);
    }
    100%{
        opacity: 1;
        transform: rotateX(0);
    }
}
最后，对于lte9其实也可以采用js来实现动画，这里就不作讨论了。总之，css3动画是个比较丰富的课题，只有一步步去探索，才会实现更大的可能。