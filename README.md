# 基于点击和滑轮事件的任意缩放和平移HTML中SVG解决方案
![](https://img.shields.io/badge/license-MIT-success.svg)
[![](https://img.shields.io/badge/Blog-SL_World-orange.svg)](https://blog.csdn.net/SL_World)
> SVG概述：`SVG`是**可缩放矢量图形**(Scalable Vector Graphics)，它基于可扩展标记语言(`XML`)，用于描述二维**矢量图形**的一种**图形格式**。

## 一、SVG特点简述
百度上有SVG的一大堆介绍，这里就不细细说明，仅摘出重点，SVG主要特点是：

 1. **不失真**——因为是矢量图
 2. 支持**任意缩放**——可用于地图存储
 3. 存储介质是**XML**——所以内部信息可被修改(如Notepad++)

## 二、SVG嵌入HTML页面的常用方式
SVG嵌入HTML的方式有很多种，以下仅介绍**两种方式**，因为只有这两种方式目前支持**svg-pan-zoom开源库**，欲了解其余方式请见底部参考文献[6]。
##### ①使用`<embed>`标签嵌入
```js
<body>	
	...
    <embed src="name.svg" class="..." >
    ...
</body>
```
##### ②使用`<object>`标签嵌入
```js
<body>	
	...
    <object data="name.svg" class="..." ></object>
    ...
</body>
```
## 三、HTML中SVG的任意缩放和平移实现
此处使用GitHub中的一个js开源库**svg-pan-zoom library**，使用其中的`svg-pan-zoom.js`文件（对于该库的详细使用方法请见底部参考文献[1]）。
##### ①引入js文件
```js
<head>
	...
    <script src="jquery.js" type="text/javascript"></script>
    <script src="svg-pan-zoom.js"></script>
    ...
</head>
```
##### ②在HTML中嵌入SVG
```js
<body>	
	...
    <object data="bada.svg" class="show-style" onload="zoom(this)"></object>
    ...
</body>
```
##### ③加入js脚本
```js
<body>	
	...
    <script type="text/javascript">
        function zoom(obj) {
            // 此处获取的元素Id是SVG文件中的<g>标签的id值
            $(obj.getSVGDocument().getElementById('Logo')).show();
            svgPanZoom(obj, {
                zoomEnabled: true,         //开启缩放功能
                controlIconsEnabled: true  //开启右下角缩放控件功能
            });
        }
        $('object').attr('onload', 'zoom(this)');
    </script>
    ...
</body>
```
其中`getElementById('Logo')`获取的元素**Id**是SVG文件中的<g>标签的id值`Logo`。
  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408195059908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NMX1dvcmxk,size_16,color_FFFFFF,t_70)

鼠标滑轮缩放效果如下，**平移则使用鼠标拖拽**即可。另外，设置了`controlIconsEnabled: true`则会显示如下图**右下角**的`RESET`**控件组**，可点击进行**缩放**和**还原**，无需专门编写该功能：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019040819451498.gif)

## 四、`svg-pan-zoom.js`源码的参数修改
在js文件约`501`行有如下默认配置选项参数，可通过**修改参数**实现**自定义效果**：
```js
var optionsDefaults = {
  viewportSelector: '.svg-pan-zoom_viewport' // Viewport selector. Can be querySelector string or SVGElement
, panEnabled: true // enable or disable panning (default enabled)
, controlIconsEnabled: false // insert icons to give user an option in addition to mouse events to control pan/zoom (default disabled)
, zoomEnabled: true // enable or disable zooming (default enabled)
, dblClickZoomEnabled: true // enable or disable zooming by double clicking (default enabled)
, mouseWheelZoomEnabled: true // enable or disable zooming by mouse wheel (default enabled)
, preventMouseEventsDefault: true // enable or disable preventDefault for mouse events
, zoomScaleSensitivity: 0.3 // Zoom sensitivity
, minZoom: 1 // Minimum Zoom level
, maxZoom: 60 // Maximum Zoom level
, fit: true // enable or disable viewport fit in SVG (default true)
, contain: false // enable or disable viewport contain the svg (default false)
, center: true // enable or disable viewport centering in SVG (default true)
, refreshRate: 'auto' // Maximum number of frames per second (altering SVG's viewport)
, beforeZoom: null
, onZoom: null
, beforePan: null
, onPan: null
, customEventsHandler: null
, eventsListenerElement: null
, onUpdatedCTM: null
}
```
其中最主要的就是如下三个参数，分别表示**缩放灵敏度**和**最小/大缩放水平**：
```js
zoomScaleSensitivity: 0.3  // 缩放灵敏度[0,1],越大缩放越快
minZoom: 0.5               // 最小缩放水平
maxZoom: 60                // 最大缩放水平
```
【参考文献】：
[\[1\] GitHub.svg-pan-zoom library.](https://github.com/SparksFly8/svg-pan-zoom)
[\[2\] Microsoft-Doc.How to Zoom and Pan with SVG.](https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/samples/gg589508%28v=vs.85%29)
[\[3\] 理解SVG坐标系和变换：视窗,viewBox和preserveAspectRatio.](https://www.w3cplus.com/html5/svg-coordinate-systems.html)
[\[4\] 做一个具有异步加载特性的echarts-vue组件(懒加载).](https://segmentfault.com/a/1190000011230007#articleHeader9)
[\[5\] echarts关系图异步加载数据.](https://blog.csdn.net/qq_37321253/article/details/77519802)
[\[6\] 菜鸟教程.SVG在HTML页面.](http://www.runoob.com/svg/svg-inhtml.html)
