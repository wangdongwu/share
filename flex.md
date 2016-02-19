title: 移动端适配方案 && flexbox
speaker: 王东武
transition: move
files: /js/demo.js,/css/demo.css,/js/zoom.js
theme: dark


[slide]
## 背景

1. 开发移动端H5页面
2. 面对不同分辨率的手机
3. 面对不同屏幕尺寸的手机

[slide]
## 视觉稿
>  视觉稿要遵守标准 {:&.flexbox.vleft}

----
1. 选取一款手机的手机的屏幕宽高作为基准(常用：iPhone 6 375x667)
2. 对于高清屏幕(retina)(如: dpr=2)，为了达到高清效果,视觉稿的画布大小会是基准的2倍(750×1334)

> 高清屏是为了提升显示相同内容时的画面精细程度，而不是为了显示更多的内容。

[slide]
## 认识一些概念
* 物理像素(physical pixel)
    * 一个物理像素是显示器(手机屏幕)上最小的物理显示单元，在操作系统的调度下，每一个设备像素都有自己的颜色值和亮度值。
* 设备独立像素(density-independent pixel)
    * 计算机坐标系统中得一个点，这个点代表一个可以由程序使用的虚拟像素(比如: css像素)，然后由相关系统转换为物理像素。
* 设备像素比(device pixel ratio )
    * 设备像素比(简称dpr)定义了物理像素和设备独立像素的对应关系

----

> 设备像素比 = 物理像素 / 设备独立像素 // 在某一方向上 x||y  {:&.flexbox.vleft}

[slide]
## 一张图展示物理像素和设备独立像素的区别
![](http://b0.hucdn.com/party/default/upload_4426d9d953e69b596486c282ecafcf6f_943x549.gif)

> 在普通屏幕下，1个css像素 对应 1个物理像素(1:1)。 在retina 屏幕下，1个css像素对应 4个物理像素(1:4)。

[slide]
## 获取DPR
1. **javascript** window.devicePixelRatio获取到当前设备的dpr
2. **css**  -webkit-device-pixel-ratio，-webkit-min-device-pixel-ratio和 -webkit-max-device-pixel-ratio进行媒体查询

[slide]
## 位图像素
1. 一个位图像素是栅格图像(如：png, jpg, gif等)最小的数据单元  {:&.flexbox.vleft}

2. 理论上，1个位图像素对应于1个物理像素，图片才能得到完美清晰的展示。在普通屏幕下是没有问题的，但是在retina屏幕下就会出现位图像素点不够，从而导致图片模糊的情况。

----

![](http://b0.hucdn.com/party/default/upload_d746ad9304ff5dba41e556d02e678f7f_498x166.jpg)

> 如上图：对于dpr=2的retina屏幕而言，1个位图像素对应于4个物理像素，由于单个位图像素不可以再进一步分割，所以只能就近取色，从而导致图片模糊(注意上述的几个颜色值)。

> 所以，对于图片高清问题，比较好的方案就是两倍图片(@2x)

[slide]
## 普通屏幕下也用了两倍图片?
----

![](http://b0.hucdn.com/party/default/upload_2956b8ab7422d2d07a6593201c4198fb_498x166.jpg)

> 一个物理像素点对应4个位图像素点,所以通过一定的算法取色，肉眼看上去虽然图片不会模糊，但是会觉得图片缺少一些锐利度，(但还是可以接受的)

[slide]
## 多屏适配布局问题
----

 移动端布局为了适配各种手机屏幕，目前最好用的方案是使用相对单位rem

> 基于rem的原理: 针对不同手机屏幕尺寸和dpr动态的改变根节点html的font-size大小(基准值)。

<div class="columns-1">
    <pre><code class="javascript">
        rem = document.documentElement.clientWidth * dpr / 10
    </code></pre>
</div>
> 说明：
> 
> 1.乘以dpr，是因为页面有可能为了实现1px border页面会缩放(scale) 1/dpr 倍(如果没有，dpr=1)

> 2.除以10，是为了取整，方便计算(理论上可以是任何值)

[slide]
## 计算出基准值rem
<div class="columns-1">
    <pre><code class="javascript">
var dpr, rem, scale;
var docEl = document.documentElement;
var fontEl = document.createElement('style');
var metaEl = document.querySelector('meta[name="viewport"]');

dpr = window.devicePixelRatio || 1;
rem = docEl.clientWidth * dpr / 10;
scale = 1 / dpr;
// 设置viewport，进行缩放，达到高清效果
metaEl.setAttribute('content', 'width=' + dpr * docEl.clientWidth + ',initial-scale=' + scale + ',maximum-scale=' + scale + ', minimum-scale=' + scale + ',user-scalable=no');
// 设置data-dpr属性，留作的css hack之用
docEl.setAttribute('data-dpr', dpr);
// 动态写入样式
docEl.firstElementChild.appendChild(fontEl);
fontEl.innerHTML = 'html{font-size:' + rem + 'px!important;}';
// 给js调用的，某一dpr下rem和px之间的转换函数
window.rem2px = function(v) {
    v = parseFloat(v);
    return v * rem;
};
window.px2rem = function(v) {
    v = parseFloat(v);
    return v / rem;
};

window.dpr = dpr;
window.rem = rem;
    </code></pre>
</div>
[slide]
## 我们的目前处理方案
<div class="columns-1">
    <pre><code class="javascript">
! function(a) {
        function b() {
            a.rem = f.getBoundingClientRect().width / 16, f.style.fontSize = a.rem + "px"
        }
        var c, d = a.navigator.appVersion.match(/iphone/gi) ? a.devicePixelRatio : 1,
            e = 1 / d,
            f = document.documentElement,
            g = document.createElement("meta");
        if (a.dpr = d, a.addEventListener("resize", function() {
                clearTimeout(c), c = setTimeout(b, 300)
            }, !1), a.addEventListener("pageshow", function(a) {
                a.persisted && (clearTimeout(c), c = setTimeout(b, 300))
            }, !1), f.setAttribute("data-dpr", d), g.setAttribute("name", "viewport"), g.setAttribute("content", "initial-scale=" + e + ", maximum-scale=" + e + ", minimum-scale=" + e + ", user-scalable=no"), f.firstElementChild) f.firstElementChild.appendChild(g);
        else {
            var h = document.createElement("div");
            h.appendChild(g), document.write(h.innerHTML)
        }
        b()
    }(window);
    </code></pre>
</div>

[slide]
## 解决那些问题
----
1. border: 1px问题
2. 图片高清问题
3. 屏幕适配布局问题

[slide]
## 如何在css编码中还原视觉稿的真实宽高
----
> rem = (真实量取)px / 基准值; {:&.flexbox.vleft}

<div class="columns-1">
    <pre><code class="less">
@base: 75;
.con{
    width : 200rem/@base;
}
    </code></pre>
}
</div>

[slide]
## 字体大小问题
####字体不可以用rem，误差太大了，且不能满足任何屏幕下字体大小相同 

 
<div class="columns-1">
    <pre><code class="less">
.px2px(@name, @px){
    @{name}: round(@px / 2) * 1px;
    [data-dpr="2"] & {
        @{name}: @px * 1px;
    }
    // for mx3
    [data-dpr="2.5"] & {
        @{name}: round(@px * 2.5 / 2) * 1px;
    }
    // for 小米note
    [data-dpr="2.75"] & {
        @{name}: round(@px * 2.75 / 2) * 1px;
    }
    [data-dpr="3"] & {
        @{name}: round(@px / 2 * 3) * 1px
    }
    // for 三星note4
    [data-dpr="4"] & {
        @{name}: @px * 2px;
    }
}
//使用
.px2px(font-size, 32);
//其他也可以使用
.px2px(padding, 20);
    </code></pre>
}
</div>

> html的data-dpr属性就是之前js方案里面有提到的，这里就有用处了 {:&.flexbox.vleft}

[slide]
## flexbox用途
----
#### 能够在**未知**或者**动态尺寸**的情况下**自由分配**容器空间的布局方式

[slide]

## 注意事项
----
* 低版本安卓
    * 在使用比例伸缩时会因为盒子内容大小不等导致内容无法等分的问题，这个时候可以为这个元素添加 width: 0%; 将其原始大小设为 0
    * 旧版要求伸缩元素（ flex-item ）必须是块级元素，所以 inline 元素需要设置 display: block; 才可以正常显示
    * Flex item 里面如果有一个块元素，设置了 margin-top，会出现溢出的问题，表现就是 margin 无效。需要在这个元素上添加 overflow:hidden; 来使其正常显示
* flex-wrap在老版规范下并不支持，出于兼容性，最好避免使用（多列情况）
* text-overflow: ellipsis; 在 display: flex; 元素上是没有效果的。


