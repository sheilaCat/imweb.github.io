title: 
date: 2015-08-06 11:39:14
comments: false
---
[UI规范](/rule/ui-rule)

# 视觉规范

|优先级|需要check的项目|产品/交互/视觉|备注|
|:-----|:--------------|:-------------|:---|
|A　　　|是否有视觉规范|需提供||
|A　　　|是否有交互稿|需提供|需求评审必须 `交互评审通过，产品抄送开发侧`、`交互有变更，更新后的交互稿抄送给开发侧`|
|B　　　|任何按钮的三态：`normal`，`hover`，`active`|视觉交付时一并给出|需求评审必须|
|B　　　|文本输入框的`normal`，`hover`，`focus`三态|视觉交付时一并给出|需求评审必须|
|B　　　|列表展示不能是绝对完美的状态，需要有多行的情况|视觉交付时一并给出|需求评审必须|
|B　　　|数据展示时如果内容为空时的提示|视觉交付时一并给出|需求评审必须|
|B　　　|`标题`、`内容的字体`、`字体大小`、`行高`、`颜色`|视觉交付时一并给出| 需求评审必须|
|B　　　|各种信息提示的视觉，比如说`提示`、`错误`、`成功`等| 视觉/交互提供模版| 开发侧提供常见出错场景|
|B　　　|转菊花或者进度条视觉| 视觉/交互提供模版| 需求评审必须|
|C　　　|如果更换设计师，请尽量保持以前的细节风格跟进设计|如有变化请告知，并保持整站一致|需求评审必须|
|C　　　|如果更换设计师，请设尽量保持以前的视觉的分辨率跟进设计|如有变化请告知，并保持整站一致|需求评审必须|
|C　　　|涉及到多页面；视觉风格保持|多页面视觉风格保持一致|需求评审必须|
|C　　　|PSD文件图层整理|是否有整理得有顺序，不凌乱？|需求评审必须；由视觉童鞋把关|
|C　　　|PSD文件命名|命名有意义，能大致告诉别人视觉稿内容|需求评审必须；由产品童鞋把关|
|D　　　|多分辨率支持考虑|如果涉及到多分辨率，视觉需要考虑|需求评审必须|
|D　　　|移动端视觉分辨率|手机默认`640x960`分辨率|需求评审必须|
|D　　　|给出的视觉中常用的字体尽量用`微软雅黑`这种字体|视觉稿是否有考虑到？|部分火星文，在客户端上需用Tahoma字体，不然会乱码；开发侧提供会乱码的场景，由视觉/产品把关，替换成Tahoma字体是否可接受|
|D　　　|隐藏图层显现考虑|视觉图层需要保持完整性|需求评审必须；移动图层在交互上移走，导致下方图层展现，需要保证下方图层可用|


# HTML规范

`强制?`缩进使用 4个空格

`强制`属性使用双引号

`建议`不在自动闭合标签后加`/`

`强制`doctype使用大写
```
<!DOCTYPE html>
<html>
    <head>
    </head>
</html>
```

`强制`除非有特殊需求,一律使用UTF8编码,书写方式约定为:
```
<head>
    <meta charset="UTF-8">
</head>
```

`强制`引入css/js时,一律不加`type`
```
<link rel="stylesheet" href="main.css">
<style>
/*...*/
</style>
<script>
//...
</script>
```

`建议`img加alt

`建议`link加title

`建议?`统一顺序, 增加gzip的压缩比
```
<a href="javascript:void(0);" target="_blank" class="item" id="xxx" title="xxx">xxx</a>
<a href="javascript:void(0);" target="_blank" class="item" id="yyy" title="yyy">yyy</a>
```


# Javascript编码规范

## 1.分号

`强制`语句必须都有分号结尾，除了for, function, if, switch, try, while

## 2. 缩进

`强制` 使用空格而非Tab来缩进, 一层缩进=4个空格

## 3. 命名

`强制` 常量使用大写字符, 下划线连接
```javascript
var SECONDS_IN_A_MINUTE = 60;
obj.TEXT_WARNNING = '警告';
```

`强制` 标准变量: 驼峰
```javascript
var myCount = 1;
```

`强制` 构造函数: 驼峰且大写第一个字母
```javascript
function Point(x, y) {
    this.x = x;
    this.y = y;
}
```

`建议` 私有方法: 驼峰且加`_`前缀
```javascript
function MyClass() {
    var _privateNum;
    this.getNum = function() {
        return _privateNum;
    };
}

// 虽然不建议这么写一个对象(建议用闭包来写)
// 但如果真这么写了, 请把意图上不想暴露的变量, 用_开头
var myCounter = {
    _count: 1,
    get: function() {
        return this._count;
    }
};
```

`建议` 对布尔型的变量, 命名时加`is`,`has`,`can`前缀

`强制` 不要使用让人糊涂的命名
```javascript
var isNotError;
var isNotClosed;
```

`强制` 当出现以下字符时,统一拼写

```javascript
var iOSVersion; //iOS
var AndroidVersion; //Android
var classID,teacherID,mID; //ID
var jumpURL; //URL
var normalHTML, isXML; //HTML
var HTTPHeader; //HTTP
//... 待补充
```

`建议` 字符串常量或字面量使用时, 使用单引号而非双引号

```javascript
var str = '<span class="info">';
str += 'some infomation</span>';
```

## 4. 代码风格

`强制` 即使是单行,也需要加花括号
`正确`
```javascript
//更建议换行写
if (isUndead) {
    grabFire();
}
//一定要一行,也必须这样
if (isUndead) { grabFire(); }
```

`错误`
```javascript
if (isUndead) grabFire();
```

`强制`操作符前后要有空格分隔
```javascript
//运算符
var a = 1 + 2;
var thaco = hit + adjustment - randomFactor;

//三元操作符
var num = val ? foo() : bar();
var fn = JSON.parse ? JSON.parse : function() {
    //...
};
```

`强制`对象属性的冒号前无空格,后跟一个空格
```javascript
var myObject = {
    propA: 1
};
```

`建议`逗号位置: Last comma

`建议` (last comma)
```javascript
var foo = 1,
    bar = 2,
    baz = 3;

var obj = {
    foo: 1,
    bar: 2,
    baz: 3
};
```

`不推荐` (first comma)
```javascript
var foo = 1
  , bar = 2
  , baz = 3;

var obj = {
    foo: 1
  , bar: 2
  , baz: 3
};
```

`注意`一定不要多写逗号了
`错误`
```javascript
var list = [
    {n: 1},
    {n: 2}, //<----- 会导致IE报错, GCC默认参数压缩也会报错
];
```

`建议`function的参数括号: 前后都加一个空格, 若非匿名函数, 则名字和括号之间不再需要空格
```javascript
//匿名函数, function和括号间有空格, 括号和花括号间也有空格
var fn = function (param) {
    //...
}
//带名字的函数, function和括号间有空格, 但插入的名字和括号间就无需再加空格了
function foo() {
    return "bar";
}
```

`建议`条件判断括号: 前后都加一个空格
`推荐`
```javascript
if (true) {
    //...
}

while (true) {
    //...
}

switch (v) {
    //...
}
```

`不推荐`
```javascript
if(true) {
    //...
}

while(true) {
    //...
}

switch(v) {
    //...
}
```

`建议`括号紧挨两端处不要空格, 中间有逗号, 逗号后加空格

`推荐`
```javascript
function fn(arg1, arg2) {
    //...
}
var fn = function (arg) {
    //...
}
if (true) {
    //...
} else {
    //...
}
var arr = [1, 2, 3];
```

`不推荐`
```javascript
function fn( arg1, arg2 ) {
    //...
}
var fn = function ( arg ) {
    //...
}
if ( true ) {
    //...
}
else {
    //...
}
var arr = [ 1, 2, 3 ];
```

`建议` if...else 写法
```javascript
if (condition1) {
    doSomething1();
} else if (condition2) {
    doSomething2();
} else {
    doSomethingElse();
}
```

`建议` switch...case 写法

```javascript
switch (condition) {
    case "first":
        // code
        break;

    case "third":
        // code
        break;

    default:
        // code
        break;
}
```

```javascript
switch (condition) {
    case "first":
    case "second": //上一行不用加fall though: 两个case紧挨, jshint不会报错
        // code
        break;

    case "third":
        // code
        /* falls through */
    case "fourth": //上一行必须加, 否则jshint会报错
        // code
        break;

    default:
        // code
        break;
}
```


`强制`函数参数过多时的排版: 两层缩进

`正确`
```javascript
var localMonsterRumors = getLocalGossip(inkeeper,
        localInn, numberOfClerics, pintsOfAlePurchased,
        charismaAjustment);
```

`错误`
```javascript
var localMonsterRumors = getLocalGossip(inkeeper,
                                          localInn,
                                          numberOfClerics,
                                          pintsOfAlePurchased,
                                          charismaAjustment);
```

`建议`采用临时变量来提高复杂判断或字符串拼接的可读性

`错误`
```javascript
if ( (conditionAA && conditionAB) || (conditionBA && conditionBB) ){
    //...
}

var elem = document.getElementById('charClass-' + charClass +
      + '_combatStats-' + armorClass + '-' + toHitBonus);
```

`正确`
```javascript
var conditionA = conditionAA && conditionAB;
var conditionB = conditionBA && conditionBB;
if (conditionA || conditionB) {
    //...
}

var strChar = 'charClass-' + charClass;
var strCombat = 'combatStatus-' + armorClass + '-' + toHitBonus;
var elem = document.getElementById(strChar + '_' + strCombat);
```

`建议`逻辑块 之间使用空行

## 5. 杂项

`建议`尽量使用标准方法而不是用非标准方法

例:
优先用string.charAt(3) 而不用 string[3]

`强制`不要修改内置对象的原型
主要是为了不污染原型从而对外部造成不好的影响
如`Array.prototype`,`Object.prototype`

`强制`避免 == != 的使用， 用严格比较条件 === !==

### for...in

`建议`对数组遍历时, 用下标的for循环而非for...in

`注意`使用for...in时要注意利用hasOwnProperty排除掉可能的原型污染干扰

### with,eval

`强制`如非特殊情况, 不允许使用with,eval

### 多行字符串

`强制`不要使用转义字符'\'的方式来写多行字符串
`强制`也不要使用function内注释再toString的hack来定义和使用多行字符串

## 保留字
`强制`对象的属性如果是保留字, 请务必使用引号定义,方括号引号引用
`正确`
```javascript
var example = {
    "new": function () {}
};

var fn = example['new'];
```

`错误`
```javascript
var example = {
    new: function () {}
};

var fn = example.new;
```

## 注释

单行注释优先

优先使用单行注释(即使是需要写多行), 除了以下情况

优先使用多行注释的情况

 - fileoverview / constructors
 - public method

多行注释
```javascript
/**
 * this method is ...
 * @param {Object} ...
 * @return {Object} ...
 */
```

## //todo: 大块注释怎么写

## //todo: switch规避jshint的一种特殊情形


# CSS规范

## 黄金法则
`GOAL` 不管多少人同时参与编码，所有代码都应该看上去像是一个人编写的一样。

## 语法

`强制` 使用四个空格的 soft tabs
> 这是保证代码在各种环境下显示一致的唯一方式。
> 暂时可使用tab代替4空格，未来我们会通过工具自动转换

`强制` 使用组合选择器时，保持每个独立的选择器占用一行

`强制` 声明块的右括号应该另起一行

`强制` 每条声明 : 后应该插入一个空格

`强制` 每条声明应该只占用一行来保证错误报告更加准确

`强制` 所有声明应该以分号结尾。
> 虽然最后一条声明后的分号是可选的，但是如果没有他，你的代码会更容易出错。

`强制` 逗号分隔的取值，都应该在逗号之后增加一个空格

`强制` 尽量使用十六进制颜色代码 #ffffff 来写颜色，除非是rgba

`建议` 尽量不使用需要特定雪碧图合成方案的css背景方案
> 便于自动化雪碧图合并

`建议` 尽量使用Pixels，而非Ems
> Ems应当更多用在line-height中

`建议` 使用[KSS](https://github.com/kneath/kss)对公共模块写文档

`建议` 使用[.editorconfig](http://editorconfig.org/)保证缩进、换行等统一

`兼容` 尽量不使用rgba、background-size等不兼容IE6的css属性

`兼容` 尽量不使用input[type="text"]、a > b、.c.d等不兼容IE6的css选择器

```css
/* Bad CSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0,0,0,0.5);
  box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* Good CSS */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin: 0 0 15px;
  background-color: rgba(0, 0, 0, .5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

## 类命名

`强制` 全小写 中划线`-`连接
- 不要使用下划线和 camelCase命名
- 短划线应该作为相关类的自然间断。如:`.btn` 和 `.btn-danger`

`强制` js-前缀样式应当只用于js，is-前缀代表即在js又在css使用的css
> css层叠样式中不应当包含js-前缀的class

`强制` 用ie6-前缀表明，对ie6专用的兼容样式

`建议` Class 的命名应该尽量短，也要尽量明确
> 避免过度使用简写。如: .btn 可以很好地描述 button，但是 .s 不能代表任何元素

## IDs VS Class

`强制`对于在单独页面只出现一次的东西，使用IDs，否则使用类，不确定也使用类

## 选择器

`强制` 确保一个选择器只出现一个ID
> 规则#header .search #quicksearch { ... } 被视为有害的。

`强制` 强制 disabled, mousedown, danger, hover, selected, 和active必须跟在一个命名空间下使用
> 例如：button.selected

## SASS

`强制` 所有scss文件开头都应当是@charset "utf-8";
> 这样可以解决编码问题

`强制` @extend置于块状声明头部

`建议` @include置于块状声明头部，@extend之后

`建议` 避免大于3层的嵌套
> 这样能有效避免过于具体的选择器

# 其他规范

## 编辑器

`强制`配置tab宽度: 4
`强制`配置使用soft tab
`强制`设置文件编码为UTF-8
`建议`自动删除行尾空白
`建议`文件结尾自动加空白行

`建议`使用`.editorconfig` //TODO:sample

## 文件组织
`待讨论`
 
 - html,js,css,img如何放置
 - 404 / qqlive / panel / panle-tabX / editor /...
 - require入口文件命名约定
 - sass入口文件约定
 - template约定
 - 其他资源
    - 
    - compontents
    - test
    - mock data
    - tools
    - docs
    - build, important shell script, config, ...