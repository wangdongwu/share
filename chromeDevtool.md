[slide]
#chrome devtool
<small>演讲:王东武</small>

[slide]
#包含 {:&.flexbox.vleft}
1. 调试
2. 技巧
3. 插件

[slide]
#调试 {:&.flexbox.vleft}
1. 快速查找文件
2. JavaScript调试
3. HTML && CSS 调试

[slide]
#查找文件 {:&.flexbox.vleft}

> 快捷键类似sublime text

1. 根据文件名快速查找 (ctrl + p || ctrl + o)
2. 根据特定文件内容查找 (ctrl + shift + f)
3. 快速跳转到特定行 (ctrl + g)
4. 在文件内查找特定内容 (ctrl + f)

[slide]
#JavaScript调试 {:&.flexbox.vleft}
1. 设置断点
	- console panel 错误提示
	- debugger
	- sources 面板设置断点
	- DOM 断点
	- XHR 断点
	- 事件 断点

2. 断点用途
	- 分步查看代码运行细节,发现出错原因
	- 写测试代码，了解当前程序的执行环境
	- 改变程序执行条件，满足自己的需要
	- 查看当前堆栈
	- 查看当前可用变量

[slide]
# HTML && CSS 调试 {:&.flexbox.vleft}
1. HTML
	- 快速在ele panel 定位DOM节点
		- 右键
		- console panel $获取节点，右键在elements panel 显示
		- find (ctrl + f)特定内容
		- console 检索 inspect(node)
	
	- 动态改变DOM 节点 && 属性
	- 强制改变元素状态(hover、active)
2. CSS 
	- 快速添加样式
	- 拷贝样式
	- 上下键 微调样式
	- 颜色选择器
	- 修改颜色显示格式(shift + click)
	
[slide]
#技巧 {:&.flexbox.vleft}

1. 保存 console 面板记录
2. 保存 network 面板记录
3. 过滤network面板
4. 关闭缓存 (客户端)
5. sources 面板代码格式化(css && javascript)
6. 快速修改resource 面板内数据
7. $()、$$()选择器 $_上次运行结果的临时变量
8. getEventListeners(node) 获取当前节点上注册了哪些事件
9. console.tabel() 查看对象详情
10. console.count('test') 获取某段代码运行次数
11. console.time('test');console.timeEnd('test') 测试代码片段运行时间

[slide]
#插件 {:&.flexbox.vleft}
1. 红杏
1. 二维码生成器
2. axure rp extension for chrome
3. web前端开发助手
4. save to pocket
5. 有道词典划词翻译

