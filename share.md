title: 两周初感受&&细节问题讨论
speaker: 王东武
transition: move
files: /js/demo.js,/css/demo.css,/js/zoom.js
theme: moon
[slide]
#一周初感受&&细节问题讨论
<small>演讲:王东武</small>

[slide]
##包含哪些内容？
----
* HTML规范(几个方面)
* css布局浅析
* JavaScript规范(几个小方面)
* git 
* 开发效率工具

[slide]
##HTML规范`(易忽略的点)`

[slide]
##文档类型
* DOCTYPE的作用
    - 怪癖模式的开关(*)
    - W3C效验
* why
    - HTML5文档类型具备前后兼容的特质
    - 拥抱HTML5
    - 简单好记忆，省流量
 
推荐  <br/>
> 在文档doctype申明之前，不允许加上任何非空字符，任何出现在doctype申明之前的字符都将使得你的HTML文档进入非标准模式


```html
        <!DOCTYPE html>
```
不推荐
```html
        <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

[slide]
## css && javascript 引入
----
推荐
```html
<link rel="stylesheet" href="//www.beibei.com/main.css"/>

<script></script>
```
不推荐
```html
<link rel="stylesheet" href="//www.beibei.com/main.css" type="text/css"/>

<script type="text/javascript"></script>
```


省略样式表与脚本上的 type 属性。鉴于 HTML5 中以上两者默认的 type 值就是 text/css 和 text/javascript，所以 type 属性一般是可以忽略掉的。甚至在老旧版本的浏览器中这么做也是安全可靠的。`link可适当的指定ref`

[slide]
##ref
* <a target="_blank" href="http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml">http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml</a>
* <a target="_blank" href="https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml">https://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml</a>
* <a target="_blank" href="http://codeguide.bootcss.com">http://codeguide.bootcss.com</a>
###google 规范网上都有相应的`中文版`自行google
<a href="http://docs.kissyui.com/1.4/source/tutorials/style-guide/google/javascriptguide.xml" target="_blank">http://docs.kissyui.com/1.4/source/tutorials/style-guide/google/javascriptguide.xml</a>

[slide]
[subslide]
##css 定位机制
---
* 正常流(Normal flow) {:&.bounceIn}
    1. 块级格式化上下文(Block formatting contexts)
    2. 行内格式化上下文(Inline formatting contexts)
    3. 相对定位 (Relative positioning)
    
* Floats model 脱离文档流(taken out of the flow and shifted to the left or right as far as possible)
* Absolute positioning 完全脱离文档流(absolute,fixed)(a box is removed from the normal flow entirely (it has no impact on later siblings))

<a target="_blank" href="#http://www.w3.org/TR/CSS21/visuren.html#block-formatting">参考</a>
<p><a href="http://codepen.io/wangdongwu/pen/MYxMEW" target="_blank">demo</a></p>
[/subslide]
[slide]
## 正常流
----

1. 正常流中的盒必定位于某一格式化上下文中，正常流中有两种格式化上下文：`块级格式化上下文`(block formatting context，简称BFC）和`行内格式化上下文`（inline formatting context,简称IFC),在块级格式化上下文中，盒呈纵向排布，在行内格式化上下文中，盒则呈横向排布。

2. 正常流中的盒分为`块级`与`行内级`两种，`任何一个行内级盒都不能够直接被放入块级格式化上下文中`。如果有一个HTML元素生成了一个行内盒，而其所在的`上下文是块级的话`，那么应当为它生成一个`匿名块级盒`，`匿名块级盒会在内部生成行内格式化上下文`
总结:行内级元素->行内格式化上下文->行内级盒->块级元素->块级盒->块级上下文

```html
<div>
    <span>hi</span>,I am a front-end developer
    <p>Good afternoon</p>
</div>
```

    
###元素的display属性会决定盒是行内级还是块级：<br />
    * block, table, flex, grid, list-item 为块级
    * inline, inline-block, inline-table, inline-flex, inline-grid 为行内级

[slide]
## (半)脱离文档流 && 完全脱离文档流
----
{:&.moveIn}
* 总结:(半)脱离文档流--其后面盒子在定位时将无视这个元素的存在，`但其他盒子内的文本依然会为这个元素让出位置，环绕在周围`。

* 而对于使用absolute positioning脱离文档流的元素，`其他盒子与其他盒子内的文本`都会无视它。
[subslide]
[/subslide]
[slide]
##`块级格式化上下文(BFC)` && `行内格式化上下文(IFC)`

[slide]
##JavaScript

----
不推荐
```javascript
 $('#alert-tip').on('click', 'button.close', function(){
        
            ...

        });
```
推荐
```javascript
 var someFunction = function(){};
 $('#alert-tip').on('click', 'button.close', someFunction);

```
不推荐
```javascript
 $('#alert-tip').click(someFunction);
```
推荐
```javascript
 $('#alert-tip').on('click', 'button.close',someFunction);
```

[slide]
## git 
----
1. 演示
2. res <br/>
    http://gitready.com/ <br/>
    http://git-scm.com/book/zh/v1 <br/>
    https://www.atlassian.com/git/ <br/>
    http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/index.html <br/>
    learn branch http://pcottle.github.io/learnGitBranching/


[slide]
---
##开发效率
* 开发工具 && 插件{:&.fadeIn}
    - webstorm && sublime text
    - snippet、code Format 、jshint
* shell
    - (mac)oh-my-zsh https://github.com/robbyrussell/oh-my-zsh
    - (mac)fish shell http://fishshell.com/
    - (window) Cygwin64 https://cygwin.com/install.html
    - (window) http://babun.github.io/
* 解决问题
    - 组内群
    - 搜索 https://duliziyou.com/、http://www.glgoo.com/