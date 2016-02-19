title: node.js 模块
speaker: 王东武
transition: move
files: /js/demo.js,/css/demo.css,/js/zoom.js
theme: dark

[slide]
# 模块
1. 什么是模块
2. 如何创建并加载模块
3. 如何创建一个包
4. 如何使用包管理器
5. 模块的加载机制

[slide]
# 什么是模块
>  应用程序的基本组成部分，文件和模块是一一对应的 <br> 
>  模块可以是js文件、json文件、编译过的c/c++拓展或者一个文件夹<br>
>  遵循commonJS 规范

[slide]

# 模块类型 
1. 核心模块 (官方提供的编译成二进制代码的模块 如:fs、http...)
2. 文件模块 (个人定制 myUtil、开源 express、...)

[slide]

# 创建并加载模块

1. exports || module.exports 模块公开的接口
2. require 从外部获取模块的接口

-----

> 注意:<br>
> 1.module.exports 与 exports关系 <br>
> 2.单次加载 <br>
> 3.require 是同步io

[slide]

# module.exports 与 exports关系
<div class="columns-2">
<pre><code class="javascript">
// example 1
var moduleA = function(){
    console.log('moduleA')
};
exports.moduleA = moduleA;
//use
require('./example').moduleA();
</code></pre>
<pre><code class="javascript">
// example 2
var moduleA = function(){
    console.log('moduleA')
};
module.exports = moduleA;
//use
require('./example')();
</code></pre>
</div>

<div class="columns-2">
<pre><code class="javascript">
// example 3
var moduleA = function(){
    console.log('moduleA')
};
var moduleB = function(){
    console.log('moduleB')
}
exports.moduleA = moduleA;
module.exports = moduleB;

>> 输出什么
</code></pre>
<pre><code class="javascript">// example 4
var module = {}
var exports = module.exports = {}
module.exports.hello = "Bonjour"
console.log(module.exports) 
// Object {hello: "Hello"}
console.log(exports) 
// Object {hello: "Hello"}
exports = null;
console.log(module.exports)
// Object {hello: "Hello"}
console.log(exports) // null
</code></pre>
</div>
----
<div>
</code></pre>
<pre><code class="javascript">// example 5
exports = module.exports  = function(){
    console.log('a');
}
exports.b = function(){
    console.log('b');
}
</code></pre>
</div>

----
[slide]

1. Node不允许重写 exports
2. 对外提供单个变量、函数或者对象用 module.exports
3. 对外提供多个...用exports
4. 如果同时包含exports 和 module.exports，exports会被覆盖

----

> exports只是对module.exports的一个全局引用<br>exports最初只是一个可以添加属性的空对象变量<br>
exports会在模块执行结束后释放，但module会不<br>
exports.module 只是module.exports.module的简写<br>如果把exports设定为别，就打破了module.exports和exports之间的引用关系

[slide]

# 单次加载

----

> require加载模块和new Object类似，但实际上有本质的区别<br>
> require不会重复加载模块，无论require多少次，获得都是同一个 <br>
> 因为 Node.js 通过文件名缓存所有加载过的文件模块,而不是require提供的参数

----

<div>
</code></pre>
<pre><code class="javascript">
# index.js 
var util = require('./util');
var example = require('./example');

console.log(util());

# example.js
var util = require('./util');
    util();

# util.js
var counter = 0;
module.exports = function(){
    return ++counter;
}

>> node index.js //输出什么
</code></pre>
</div>

[slide]
# require 同步IO

require 本质上是一个同步IO

1. 文件在顶端引入
2. I/O密集型的地方不要用require(如：避免在每个http请求里使用require)

[slide]

# 创建包


包是在模块基础上更深一步的抽象，它将某个独立的功能封装起来<br>用于发布、更新、依赖管理和版本控制<br>
{:&.flexbox.vleft}
Node.js 根据CommonJS实现的包机制

----
{:&.flexbox.vleft}
1. package.json必须在包的顶层目录下
2. 二进制文件应该在bin目录下
3. javascript文件在lib目录下
4. 文档在doc目录下
5. 单元测试在test目录下

![](http://b0.hucdn.com/party/default/upload_81ffb30f853d70b3db6038c65201a128_261x299.png){:&.flexbox.vleft}

[slide]
# package.json

<div>
</code></pre>
<pre><code class="json">
{
  "name": "browserify",
  "version": "12.0.2",
  "description": "browser-side require() the node way",
  "main": "index.js",
  "bin": {
    "browserify": "bin/cmd.js"
  },
  "repository": {
    "type": "git",
    "url": "git+ssh://git@github.com/substack/node-browserify.git"
  },
  "keywords": [
    "browser"
  ],
  "dependencies": {
    "JSONStream": "^1.0.3"
  },
  "devDependencies": {
    "backbone": "~0.9.2",
  },
  "author": {
    "name": "James Halliday"
  },
  "scripts": {
    "test": "tap test/*.js"
  },
  "license": "MIT",
  "gitHead": "d6cd17b6a467bb53ce053cb22fc1525617d76376",
  "bugs": {
    "url": "https://github.com/substack/node-browserify/issues"
  },
  "homepage": "https://github.com/substack/node-browserify#readme"
}
</code></pre>
</div>

[slide]

# 包管理 npm
本地模式和全局模式

- npm install(i) xx || npm install(i) -g xx
    - 为什么要使用全局模式？因本地模式不会注册path环境变量
    - 为什么要要使用本地模式？为了处理不同版本的依赖
    - 当包作为工程运行时的一部分时，通过本地模式获取<br>如果要在命令行下使用，则使用全局模式安装。
- npm link 本地包和全局包之间创建符号链接
    - npm link express 本地包中发现指向全局的符号链接,这样可以把全局包当本地包使用
    - npm link (在本地包 package.json所在目录中执行) 将本地包连接到全局，开发包时方便在不同的工程间进行测试

[slide]

# 模块加载机制

----
1. 如果 some_module  是一个核心模块，直接加载，结束。 
2. 如果 some_module 以“ / ”、“ ./ ”或“ ../ ”开头，按路径加载  some_module ，结束。 
3. 假设当前目录为 current_dir，按路径加载 current_dir/node_modules/some_module。  
    - 如果加载成功，结束。
    - 如果加载失败，令current_dir为其父目录。 
    - 重复这一过程，直到遇到根目录，抛出异常，结束。
到工程共同依赖的模块时，就需要向父目录上溯了


