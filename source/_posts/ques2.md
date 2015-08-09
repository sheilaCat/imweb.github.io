title: Ques前端组件化体系（二）
date: 2015-08-01 16:35:29
tags: 工具建设
---
紧接上文：http://imweb.io/topic/55abbc5a6ee095884b704c2e
组件嵌套
当组件可以嵌套，组件件可以拆成较小的颗粒，使得复用性大大提升。
下面我们是一个codeclick组件，其用途是高亮展示代码片段，点击代码弹出dialog，也就是说我们准备嵌套上面做出来的third-code和dialog组件：
定义
CSS文件：
/** 可以给组件定义一些特殊样式，但为了简单我们什么也不做 **/
模版文件：
<div>
    <third-code q-ref="code">
        <content></content>
    </third-code>
    <dialog q-ref="dialog">
        <header>欢迎使用Ques</header>
        <article>你点击了代码</article>
    </dialog>
</div>
Javascript文件：
var $ = require('jquery');

module.exports = {
    data: {},
    ready: function () {
        var code = this.$.code,
            dialog = this.$.dialog;
        // 点击代码，弹出dialog
        $(code.el).on('click', function () {
            dialog.show();
        });
    }
};