# web布局方案

> 静态布局，自适应布局，流体式布局，响应式布局,基于rem的布局,弹性盒模型
>
> [HTML 主流布局区别差异动态演示 流式 自适应 响应式 静态](http://wow.techbrood.com/fiddle/fork?id=25477) \([http://wow.techbrood.com/fiddle/fork?id=25477\](http://wow.techbrood.com/fiddle/fork?id=25477%29\)

## 静态布局

> **静态布局：元素不变的布局。**
>
> 布局特点：窗口缩小后内容被遮挡时，拖动滚动条显示布局
>
> 设计方法：
>
> PC：居中布局，所有样式使用绝对宽度，高度
>
> 移动设备：另外建立移动网站，以m.域名为域名

## **自适应布局**

> **自适应布局：创建多个静态布局，每个静态布局对应一个屏幕分辨率范围**
>
> 布局特点：改变屏幕分辨率可以切换不同的静态局部，在每个静态布局中，元素不发生变化，相当于静态布局的一个系列
>
> 设计方法：使用媒体查询功能

## **流体式布局**

> **流体式布局:元素宽度按照分辨率宽度进行长度、宽度的调整，但布局不变**
>
> 缺点：如果屏幕尺度跨度过大，那么在相对原始设计而言过小或过大的屏幕上不能正常显示
>
> 流体布局实现，就是使用百分比来设置元素的宽度，元素的高度按实际高度写固定值，流体布局中，元素的边线无法用百分比，可以使用样式中的计算函数 calc\(\) 来设置宽度，或者使用 box-sizing 属性将盒子设置为从边线计算盒子尺寸。
>
> **calc\(\)**  
> 可以通过计算的方式给元素加尺寸，比如： width：calc\(25% - 4px\);
>
> **box-sizing**  
> 1、content-box 默认的盒子尺寸计算方式  
> 2、border-box 置盒子的尺寸计算方式为从边框开始，盒子的尺寸，边框和内填充算在盒子尺寸内

## 响应式布局

> **响应式布局：创建多个流体式布局，分别对应一个屏幕分辨率范围**
>
> 特点：分别为不同的屏幕分辨率定义布局，同时，在每个布局中，应用流式布局的理念，即页面元素宽度随着窗口调整而自动适配
>
> 响应式布局就是使用媒体查询的方式,创建多个元素宽度是相对的的布局理想的响应式布局是指的对PC/移动各种终端进行响应的

相应布局的伪代码如下：

```
.a{
   height: 200px;
   display: inline-block;
}
/*当浏览器窗口小于=960时*/
@media (max-width:960px){
    .a{width:50%;}
}
/*当浏览器窗口小于=640时*/
@media (max-width:640px){
    .a{width:100%;}
}
/*当浏览器窗口大于等于960时*/
/*@media (min-width:960px){
    .a{width:25%;}
}*/
```

## 基于rem的布局

#### 首先，在介绍 **rem** 之前，我们先看看 **em**

> **em**是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。\(引自CSS2.0手册\)
>
> 通俗的讲就是： **1em**，就是这个元素上的 **font-size** 的值，如果元素本身没有设置 **font-size** ，就按照他的父级的 **font-size** 计算（其实本身的font-size继承了父级的font-size），我们看看代码：

```
<style type="text/css">
    .box{font-size:20px; border:1px solid red; overflow:hidden;}
    .box>.con{width:10em; height:10em; background:#000; color:#fff;}
    .box>.con_1{float:left;}
    .box>.con_2{float:right; font-size:30px;}
</style>
<div class="box">
    <div class="con con_1">con1:我没有font-size</div>
    <div class="con con_2">con2:我有font-size</div>
</div>
```

#### 现在来看看rem

> rem是CSS3新增的一个相对单位（root em，根em）。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器均已支持rem
>
> 在W3C官网上是这样描述**rem**的—— “font size of the root element \(根元素的字体大小\)” ，
>
> 也就是说，**rem**是相对于根元素`<html>`的

```
<style type="text/css">
    .box{font-size:20px; border:1px solid red; overflow:hidden;}
    .box>.con{width:10rem; height:10rem; background:#000; color:#fff;}
    .box>.con_1{float:left;}
    .box>.con_2{float:right; font-size:30px;}
</style>
<div class="box">
    <div class="con con_1">con1:我没有font-size</div>
    <div class="con con_2">con2:我有font-size</div>
</div>
```

### rem如何适配各种设备屏幕

> 通过js,根据当前浏览器可视区域大小,来动态调整页面中html的字体

```
 /* rem.js文件内容 */
(function(){
    var calc = function(){
        //获取当前文档
        var docElement = document.documentElement;
        // 判断 当前浏览器的可视区域宽度 大于 750 那么变量就是  750,否则就是当前浏览器可视区域的实际宽度
        var clientWidthValue = docElement.clientWidth > 750 ? 750 : docElement.clientWidth;
        //计算并设置当前文档的字体大小
        docElement.style.fontSize = 20*(clientWidthValue/375)+'px';
    }
    calc();
    //绑定事件,当浏览器的大小发生改变时
    window.addEventListener('resize',calc);
})();
```

## px自动转换为rem插件 CSSREM

> 一个CSS的px值转rem值的Sublime Text 3自动完成插件。
>
> 插件效果如下：

![](/assets/cssrem.gif)

##### 安装

* 下载本项目，
* 进入packages目录：Sublime Text -&gt;Preferences -&gt;Browse Packages...
* 复制下载的cssrem目录到刚才的packges目录里。
* 重启Sublime Text。

##### 配置参数

参数配置文件：Sublime Text -&gt; Preferences -&gt; Package Settings -&gt; cssrem-&gt; ... User

```
{
    "px_to_rem": 20, //px转rem的单位比例
    "max_rem_fraction_length": 6, //px转rem的小数部分的最大长度。默认为6。
    "available_file_types": [".css", ".less", ".sass",".html"]
    //启用此插件的文件类型。默认为：[".css", ".less", ".sass"]
}
```

## CSS3 弹性盒子\(Flex Box\)

> 弹性盒子是 CSS3 的一种新的布局模式。
>
> CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。
>
> 引入弹性盒布局模型的目的是提供一种更加有效的方式来对一个容器中的子元素进行排列、对齐和分配空白空间。
>
> [http://www.css88.com/tool/flexboxfroggy/\#zh-cn](http://www.css88.com/tool/flexboxfroggy/#zh-cn)



