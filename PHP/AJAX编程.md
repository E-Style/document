#HTTP服务&AJAX编程

​                  

# 第1章 AJAX编程 


## 1.1  Ajax

即 Asynchronous Javascript And XML，AJAX 不是一门的新的语言，而是对现有持术的综合利用。

 	1. 基于web标签的xhtml+css
	2. 可以使用dom进行动态的显示和交互
	3. 使用XML和XSLT(是一种用于将XML[文档](https://baike.baidu.com/item/%E6%96%87%E6%A1%A3)转换任意文本的描述语言)进行数据的交换和操作
	4. 使用XMLHttpRequest进行异步的数据查询和检索等操作。。。

本质:是在HTTP协议的基础上以异步的方式通过XMLHttpRequest对象与服务器进行通信。

作用：可以在页面不刷新的情况下，请求服务器，局部更新页面的数据；

## 1.2  异步（Asynchronous [eɪˈsɪŋkrənəs])

指某段程序执行时不会阻塞其它程序执行，其表现形式为程序的执行顺序不依赖程序本身的书写顺序，相反则为同步。 

其优势在于不阻塞程序的执行，从而提升整体执行效率。

同步：同一时刻只能做一件事，上一步完成才能开始下一步

异步：同时做多件事，效率更高

XMLHttpRequest可以以异步方式的处理程序。

##  1.3  XMLHttpRequest 

浏览器内建对象，用于在后台与服务器通信(交换数据) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。

下面是一个简单的例子

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image004.jpg)

由于XMLHttpRequest本质基于HTTP协议实现通信，所以结合HTTP协议和上面的例子我们分析得出如下结果：

###  1.3.1   请求 

HTTP请求3个组成部分与XMLHttpRequest方法的对应关系

1、请求行

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image006.jpg)

2、请求头

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image008.jpg)

**get 请求可以不设置 **

3、请求主体

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image010.jpg)

注意书写顺序

### 1.3.2   响应 

HTTP响应是由服务端发出的，作为客户端更应关心的是响应的结果。

HTTP响应3个组成部分与XMLHttpRequest方法或属性的对应关系。

由于服务器做出响应需要时间（比如网速慢等原因），所以我们需要监听服务器响应的状态，然后才能进行处理。

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image012.jpg)

**onreadystatechange 是   Javascript 的事件的一种，其意义在于监听 XMLHttpRequest 的状态 **

1、获取状态行（包括状态码&状态信息）

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image014.jpg)

2、获取响应头

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image016.jpg)

3、响应主体

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image018.jpg)

我们需要检测并判断响应头的MIME类型后确定使用request.responseText或者request.responseXML

### 1.3.3   API 详解

xhr.open() 发起请求，可以是get、post方式

xhr.setRequestHeader() 设置请求头

xhr.send() 发送请求主体get方式使用xhr.send(null)

xhr.onreadystatechange = function () {} 监听响应状态

readstate 属性有五个状态：

- xhr.readyState = 0时，（未初始化）还没有调用send()方法
- xhr.readyState = 1时，（载入）已调用send()方法，正在发送请求
- xhr.readyState = 2时，（载入完成）send()方法执行完成，已经接收到全部响应内容
- xhr.readyState = 3时，（交互）正在解析响应内容
- xhr.readyState = 4时，（完成）响应内容解析完成，可以在客户端调用了

`不用记忆状态，只需要了解有状态变化这个概念`

xhr.status表示响应码，如200

xhr.statusText表示响应信息，如OK

xhr.getAllResponseHeaders() 获取全部响应头信息

xhr.getResponseHeader('key') 获取指定头信息

xhr.responseText、xhr.responseXML都表示响应主体

**注GET和POST请求方式的差异（面试题）**

1、GET没有请求主体，使用xhr.send(null)

2、GET可以通过在请求URL上添加请求参数

3、POST可以通过xhr.send('name=itcast&age=10')

4、POST需要设置

Content-type:application/x-www-form-urlencoded

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image020.jpg)

5、GET大小限制约4K，POST则没有限制

问题？如何获取复杂数据呢？

## 1.4  XML extensible markup language

XML是一种标记语言，很类似HTML，其宗旨是用来传输数据，具有自我描述性（固定的格式的数据）。

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image022.jpg)

### 1.4.1   语法规则

1、必须有一个根元素 

2、标签名称不可有空格、不可以数字或.开头、大小写敏感

3、不可交叉嵌套

4、属性双引号（浏览器自动修正成双引号了）

5、特殊符号要使用实体

6、注释和HTML一样

虽然可以描述和传输复杂数据，但是其解析过于复杂并且体积较大，所以实现开发已经很少使用了。

## 1.5  JSON

即 JavaScript Object Notation，另一种轻量级的文本数据交换格式，独立于语言。

### 1.5.1   语法规则

1、数据在名称/值对中

2、数据由逗号分隔(最后一个健/值对不能带逗号)

3、花括号保存对象方括号保存数组

4、使用双引号 

### [1.5.2   JSON]()解析

JSON数据在不同语言进行传输时，类型为字符串，不同的语言各自也都对应有解析方法，需要解析完成后才能读取

1、Javascript 解析方法

JSON对象   JSON.parse()、 JSON.stringify()；

2、PHP解析方法

json_encode()、json_decode()

总结：JSON体积小、解析方便且高效，在实际开发成为首选。

 

## 1.6  封装AJAX工具函数

为了提升我们的开发效率，我们自已将XMLHttpRequest封装成一个函数。

## 1.7  jQuery中的Ajax

jQuery为我们提供了更强大的Ajax封装

$.ajax({}) 可配置方式发起Ajax请求

$.get() 以GET方式发起Ajax请求

$.post() 以POST方式发起Ajax请求

$('form').serialize()序列化表单（即格式化key=val&key=val）

url 接口地址

type 请求方式

timeout 请求超时

dataType 服务器返回格式

data 发送请求数据

beforeSend:function () {} 请求发起前调用

success 成功响应后调用

error 错误响应时调用

complete 响应完成时调用（包括成功和失败）

**jQuery Ajax介绍**

**http://www.w3school.com.cn/jquery/jquery_ref_ajax.asp**

## 1.8  案例练习

1、Loading状态

2、禁止重复提交

3、表单处理

4、数据验证

 

 

## 1.9  接口化开发

请求地址即所谓的接口，通常我们所说的接口化开发，其实是指一个接口对应一个功能，并且严格约束了请求参数和响应结果的格式，这样前后端在开发过程中，可以减少不必要的讨论，从而并行开发，可以极大的提升开发效率，另外一个好处，当网站进行改版后，服务端接口只需要进行微调。


# 第2章 模板引擎

Js插件 template_native.js

作用：渲染数据时  代替 拼接字符串的操作 

## 2.1  流行模板引擎

**BaiduTemplate**：http://tangram.baidu.com/BaiduTemplate/

**ArtTemplate**：http://aui.github.io/art-template/zh-cn/

https://aui.github.io/art-template/
**velocity.js**：https://github.com/shepherdwind/velocity.js/

**Handlebars**：http://handlebarsjs.com/
http://blog.jobbole.com/56689/

## 2.2  artTemplate

1、引入template-web.js

2、在{{ }}中包含js代码

3、语法：

 Js代码包含在{{ }}中

{{data.key}}

{{data['key']}}

{{a ? b : c}}

条件判断语句：

{{if value}} ... {{/if}}

{{if v1}} ... {{else if v2}} ...{{/if}}

   循环遍历：

{{each target   }}

   {{\$index}}  {{\$value}}

{{/each}}

 

# 第3章 同源&跨域

## 3.1  同源

同源策略是浏览器的一种安全策略，所谓同源是指，域名，协议，端口完全相同。

[www.jd.com--->www.taobao.com](http://www.jd.com---%3ewww.taobao.com)

[www.taobaol.com](http://www.taobaol.com)

M.taobao.com  www.zfb.com

 

## 3.2  跨域

不同源则跨域

1-不允许进行DOM操作

2-不能进行ajax请求

例如http://www.example.com/

| http://api.example.com/detail.html       | 不同源 | 域名不同       |
| ---------------------------------------- | ------ | -------------- |
| https://www.example.com/detail.html      | 不同源 | 协议不同       |
| http://www.example.com:8080/detail.html  | 不同源 | 端口不同       |
| http://api.example.com:8080/detail.html  | 不同源 | 域名、端口不同 |
| https://api.example.com/detail.html      | 不同源 | 协议、域名不同 |
| https://www.example.com:8080/detail.html | 不同源 | 端口、协议不同 |
| http://www.example.com/detail/index.html | 同源   | 只是目录不同   |

## 3.3  跨域方案

1、顶级域名相同的可以通过domain.name来解决，即同时设置 domain.name = 顶级域名（如example.com）

2、document.domain+ iframe

3、window.name+ iframe

4、location.hash+ iframe

5、window.postMessage()

**参考资料**

**http://rickgray.me/2015/09/03/solutions-to-cross-domain-in-browser.html**

以上几种解决方案多数只能在某一特定情形下使用，兼容性及通用性都不够好，更通用的是JSONP解决方案。**

## 3.4  JSONP

JSON with Padding

**1**、原理剖析

其本质是利用了<script src=""></script>标签具有可跨域的特性，由服务端返回一个预先定义好的Javascript函数的调用，并且将服务器数据以该函数参数的形式传递过来，此方法需要前后端配合完成。

只能以GET方式请求

## 3.5  jQuery中的JSONP

jQuery 的$.ajax() 方法当中集成了JSONP的实现，可以非常方便的实现跨域数据的访问。

dataType:'jsonp' 设置dataType值为jsonp即开启跨域访问

jsonp 可以指定服务端接收的参数的“key”值，默认为callback

jsonpCallback 可以指定相应的回调函数，默认自动生成

 

 

## 3.6  服务器端跨域 CORS跨域：

header( 'Access-Control-Allow-Origin:*' ); 

header( 'Access-Control-Allow-Origin:http://www.study.com');

**案例练习**

** **

天气接口：

<http://lbsyun.baidu.com/index.php?title=car/api/weather>

天气预报的密钥：zVo5SStav7IUiVON0kuCogecm87lonOj

百度API：

<http://apistore.baidu.com/>

密钥：

21f068f20fd5b7ca99c8908c1ae9f2bb

 

图灵机器人：http://www.tuling123.com/register/index.jhtml

# 第4章 XMLHttpRequest2.0

技术总是在实践中不断更新的，XMLHttpRequest也不例外。

## 4.1  设置超时

a)    设置超时 xhr.timeout

b)    监听超时事件 xhr.ontimeout = function () {// code}

当请求超时，此事件就会被触发

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image024.jpg)

## 4.2  FormData

a)    提供了一个新的内建对象，可用于管理表单数据

b)    首先要获取一个表单元素form

c)    然后在实例化时 new FormData(form)，将表单元素form传进去

d)    会返回一个对象，此对象可以直接做为xhr.send(formData)的参数

e)    此时我们的数据就是以二进制形式传递了

f)     注意我们这里只能以post形式传递，浏览器会自动为我们设置一个合适的请求头

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image026.jpg)

## 4.3  二进制

a)    我们上传文件是以二进制形式传递的

b)    我们可以通过表单<input type=”file”>获取到一个文件对象

c)    然后file.files[0]可以获取文件信息

d)    然后再利用var formData = new FormData() 实例化

e)    然后再利用formData.append(‘upload’, file.files[0])将文件转成二进制

f)     最后将 formData 做为xhr.send(formData)的参数

见代码示例11-3.html 和 11-3.php

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image028.jpg)

## 4.4  上传进度

a)    利用XMLHttpRequest我们可以实现文件的上传

b)    并且我们可以通过xhr.upload.onprogress = function (ev) {// code}，监听上传的进度

c)    这时我们上传的进度信息会保存在事件对象ev里

d)    ev.loaded 表示已上传的大小，ev.total表示文件整体的大小

e)    var percent = ev.loaded / ev.total

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image030.jpg)