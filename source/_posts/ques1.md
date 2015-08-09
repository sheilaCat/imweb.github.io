title: Ques前端组件化体系（一）
date: 2015-08-02 16:35:29
tags: 工具建设
---
Ques是一套组件化系统，解决如何定义、嵌套、扩展、使用组件。 项目地址：https://github.com/miniflycn/Ques
传统开发模式的痛点
无法方便的引用一个组件，需要分别引用其Javascript、Template、CSS文件
我们期望能以MV*的方式去写代码，结果发现只有Javascript是MV*
UI库打包成一坨(类似Bootstrap)，但是实际上UI库伴随产品迭代在反复变更，每次打开网站，用户依然反复下载UI库
CSS没有命名空间导致两个组件容易冲突
组件无法嵌套或者无法扩展，所以实际上组件根本无法复用
组件无法复制后可用，在组件无法嵌套或无法扩展的情况下，连定制一个组件都困难连连
每次性能优化都将代码弄的很恶心，不好维护
可能没法支持IE6，例如React、Vuejs、Polymer这些方案IE6肯定不支持
UI组件
UI组件是用来专门做UI的组件，其特点为只有模版、样式文件。系统会根据用户在页面已使用的UI组件动态引用其依赖的资源。注意，UI组件的前缀必须是ui-。
下面是一个简单的ui-button的例子：
定义
CSS文件
.ui-button {
    display: inline-block;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    touch-action: manipulation;
    cursor: pointer;
    user-select: none;
    background-image: none;
    border: 1px solid transparent;
    border-radius: 4px;
    text-transform: none;
    -webkit-appearance: button;
    overflow: visible;
    margin: 0;
}

.ui-default {
    color:#333;
    background-color:#fff;
    border-color:#ccc
}
.ui-default.focus,.ui-default:focus {
    color:#333;
    background-color:#e6e6e6;
    border-color:#8c8c8c
}
.ui-default:hover {
    color:#333;
    background-color:#e6e6e6;
    border-color:#adadad
}

// 后面可添加更多样式的按钮
模版文件
<button class="ui-button">
    <content></content>
</button>
效果
在页面上直接引用：