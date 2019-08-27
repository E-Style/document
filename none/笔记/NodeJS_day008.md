> #  NodeJS简要札记

---

- 首页（文章列表）

  - 分页

- 文章详情

- 发表新文章

  - markdown编辑器效果

- 编辑已发表文章

  

## 1.0 添加文章页面拦截未登录



### 1.1 拦截未登录请求

```javascript
const express = require('express')
const router = express.Router()

const ctrl = require('../controller/article.js')

// 监听客户端的 get 请求地址，显示 文章添加页面
router.get('/article/add', ctrl.showAddArticlePage)

module.exports = router
```



​	在 controller 文件夹中，新建 article.js

```javascript
const showAddArticlePage = (req, res) => {
  	if(!req.session.islogin) return res.redirect('/')
  
     res.render('./srticle/add.ejs', {
       	user: req.session.user,
       	islogin: req.session.islogin
     })
}

module.exports = {
  	showAddArticle
}
```





---



## 2.0 完成发表文章



### 2.1 完成基本功能

```html
<%- include('../layout/header.ejs') %>
  
  
  <div class="container">
    	<h1>发布文章页</h1>
    	<hr>
    	<form id="form">
          	<div class="form-group">
              	<label>文章标题：</label>
              	<input type="text" name="title" class="form-control">
          	</div>
          
          	<div class="form-group">
              	<label>文章内容：</label>
              	<textarea name="content" class="form-control"></textarea>
          	</div>
          
          	<div class="form-group">
              	<input type="submit" value="发表文章" class="btn btn-primary">
          	</div>
    	</form>
  </div>
  
  
<%- include('../layout/footer.ejs') %>
```



​	在npm上搜索 mditor 包

​	`npm i mditor -S`

​	在 add.ejs 页面中完善内容

```html
<link rel="stylesheet" href="/node_modules/mditor/dist/css/mditor.min.css">
<script src="/node_modules/mditor/dist/js/mditor.min.js"></script>

<textarea name="content" class="form-control" id="editor"></textarea>


<script>
	$(function() {
      	var mditor =  Mditor.fromTextarea(document.getElementById('editor'));
        
        //获取或设置编辑器的值
        mditor.on('ready',function(){
          console.log(mditor.value);
          mditor.value = '** hello **';	
        });
	})
</script>
```



### 2.2 完成最终功能

​	设计表

![artticle](images\artticle.png)



​	在 add.js 文件中发起请求

```html
<script>
	$(function() {
      	var mditor = Mditor.formTextarea(document.getElementById("editor"))
        
        $("#form").on('submit', function(e) {
          	e.preventDefault()
            $.ajax({
              	type: 'post',
              	url: '/article/add',
              	data: $("#form").serialize(),
              	dataType: 'json',
              	success: function(res) {
                  	// res 返回的结果，有一个 insertId
                  	if(res.status !== 200) return alert('发布文章失败')
                    location.href = '/artlcle/info/'+ res.insertId
              	}
            })
        })
	})
</script>
```



​	在 router文件夹中，编辑 article.js

```javascript
// 监听前端post请求
router.post('/article/add', ctrl.addArtilce)
```



​	在 views 文件夹中的 article 文件夹文件中，在form表单中增加一个隐藏域，记录用户id

```html
<input type="hidden" name="authorId" value="<%= user.id %>">
```



​	在 controller 文件夹中，编辑 article.js

```javascript
const moment = require('moment')
const conn = require('../db/index.js')

const addArtilce = (req, res) => {
  	cosnt body = req.body
    // 最好不要在服务器端获取作者的id，如果作者编写了很长的时间，session会失效
    // body.authorId = req.session.user.id
    body.ctime = moment().format('YYYY-MM-DD HH:mm:ss')
    let sql = 'inset into blog_articles set ?'
    conn.query(sql, body, (err, result) => {
      	if(err) return res.send({msg: '发表文章失败', status: 500})
        if(result.affectedRows !== 1) res.send({msg: '发表文章失败', status: 501})
        res.send({msg: '发表文章成功', status: 200, insertId: result.insertId})
    })
}

module.exports = {
  	addArtilce
}
```



### 2.3 完成详情页

​	在 router文件夹中，编辑 article.js

```javascript
// 监听展示详情页的请求
router.get('/artlcle/info/:id', ctrl.showArticleDetail)
```

​	

 	在 controller 文件夹中，编辑 article.js

​	`npm i marked -S`

```javascript
const marked = require('marked')

const showArticleDetail = (req, res) => {
  	// 获取文章id
  	const id = req.params.id
    // 根据id查询文章信息
    const sql = 'select * from blog_articles where id = ?'
    conn.query(sql, id, (err, result) => {
      	if(err) return res.send({msg: '获取文章详情失败', status: 500})
        if(result.length !== 1) return res.redirect('/')
        
        // 在调用之前，要先把 markdown 文本转为 html文本
        result[0].content = marked(result[0].content)
        
        res.render('./article/info.ejs', {
          user: req.session.user,
          islogin: req.session.islogin,
          article: result[0]
        })
    })
}

// 增加方法，之前的方法不要丢了
module.exports = {
  	showArticleDetail
}
```

​	

​	views 文件夹 article 文件夹中，添加 info.ejs

```html
<%- include('../layout/header.ejs') %>
  
  <div>
    <h1 class="text-center">
      <%= article.title %>
      <a href="#" class="btn btn-info pull-right">编辑</a>
    </h1>
    <hr>
    <div>
      	<%- article.content %>
    </div>
  </div>
  
  
<%- include('../layout/footer.ejs') %>
```





---



## 3.0 编辑功能



### 3.1 控制编辑按钮

​	views 文件夹 article 文件夹中，编辑 info.ejs

```ejs
	<h1 class="text-center">
      <%= article.title %>
      
      <% if(isLogin && user.id === article.authorId) { %>
        <a href="/article/edit/<%= article.id %>" class="btn btn-info pull-right">编辑</a>
      <% } %>
    </h1>
```



### 3.2 完成编辑文章

​	views 文件夹 article 文件夹中，添加 edit.ejs

```ejs
<%- include('../layout/header.ejs') %>

<link rel="stylesheet" href="/node_modules/mditor/dist/css/mditor.min.css">
<script src="/node_modules/mditor/dist/js/mditor.min.js"></script>
  
  <div class="container">
    	<h1>编辑文章页</h1>
    	<hr>
    	<form id="form">
          	<input type="hidden" name="id" value="articleId">
          	<div class="form-group">
              	<label>文章标题：</label>
              	<input type="text" name="title" class="form-control" value="<%= article.title %>">
          	</div>
          
          	<div class="form-group">
              	<label>文章内容：</label>
              	<textarea name="content" class="form-control" id="editor">
                  <%= article.content %>
              	</textarea>
          	</div>
          
          	<div class="form-group">
              	<input type="submit" value="保存文章" class="btn btn-primary">
          	</div>
    	</form>
  </div>
  
  	<script>
		$(function() {
          	// 初始化编辑器
          	var mditor = Mditor.formTextarea(document.getElementById("editor"))
		})
	</script>
  
<%- include('../layout/footer.ejs') %>
```



​	在 router文件夹中，编辑 article.js

```javascript
// 监听客户端 请求文章编辑页面
router.get('/article/edit/:id'. ctrl.showEditPage)

// 监听用户编辑文章
router.post('/article/edit', ctrl.editArticle)
```



​	在 controller 文件夹中，编辑 article.js

```javascript
// 显示编辑页面
const showEditPage = (req, res) => {
  	// 如果用户没有登录则不允许查看文章编辑页面
  	if(!req.session.islogin) return res.redirect('/')
    const sql = 'select * from blog_articles where id = ?'
    conn.query(sql, req.params.id, (err, result) => {
      	if(err) return res.redirect('/')
        if(result.length !== 1) return res.redirect('/')
        // 渲染详情页
        res.render('./article/edit.ejs', {
          user: req.session.user, 
          islogin: req.session.islogin,
          article: result[0]
        })
    })
}

// 编辑功能请求
const editArticle = (req, res) => {
  	let sql = 'update blog_articles set ? where id = ?'
    conn.query(sql, [req.body, req.body.id], (err, result) {
      	if(err) return res.send({msg: '修改文章失败', status: 501})
    	if(result.effectedRows !== 1) return res.send({msg: '修改文章失败', status: 502})
        
         res.send({msg: 'ok', status: 200})
    })
}

module.exports = {
  	showEditPage,
  	editArticle
}
```



​	页面的ajax请求

```javascript
$("#form").on('submit', function(e) => {
  	e.preventDefault()

	$.ajax({
      	type: 'post',
      	url: '/article/edit',
      	data: $("#form").serialize(),
      	dataType: 'json',
      	success: function(res) {
          	if(res.status !== 200) return slert('文章修改失败了')
            location.href = '/article/info/<%= article.id %>'
      	}
	})
})
```





---



## 4.0  渲染文章列表



### 4.1 渲染文章列表

​	打开 router文件夹，编辑 index.js

```javascript
const conn = require('../db/index.js')

// 展示首页页面
const showIndexPage = (req, res) => {
  	const sql = 'SELECT blog_articles.id, blog_articles.title, blog_articles.ctime, blog_users.nickname FROM blog_articles LEFT JOIN blog_users ON blog_articles.authorId = blog_users.id ORDER BY blog_articles.id DESC'
 	
    conn.query(sql, (err, result) => {
		if(err) {
          	 res.render('index.ejs', {
                user: req.session.user,
                islogin: req.session.islogin,
               	article: []
             })
		}
    })
    
    // 渲染首页页面
    res.render('index.ejs', {
      	user: req.session.user,
      	islogin: req.session.islogin,
      	article: result
    })
}

module.exports = {
  	showIndexPage
}
```



​	打开 views 文件夹，编辑 index.ejs

```ejs
<%- include('../layout/header.ejs') %>

	<h1>文章列表</h1>

	<div class="list-gorup"  style="margin: 10px;">
      	<% article.forEach(item => { %>
      		<a href="/article/info/<%= item.id %>" class="list-group-item">
              <%= item %>
              <span class="badge" style="background-color: #5bc0de">发表时间：<%= item.ctime %></span>
              <span class="badge" style="background-color: #f0ad4e">作者昵称：<%= item.nickname %></span>
      		</a>
      	<% }) %>
	</div>

<%- include('../layout/footer.ejs') %>
```



### 4.2 渲染分页的页码条和激活的页码

​	通过bootstrap加载出分页按钮

```html
    <!-- 分页区域 -->
    <nav aria-label="Page navigation">
        <ul class="pagination">
            <li>
                <a href="#" aria-label="Previous">
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
            <% for(var i = 0; i < totalPage; i++) { %>
            <li><a href="#">
                    <%= (i + 1) %></a></li>
            <% } %>
            <li>
                <a href="#" aria-label="Next">
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </ul>
    </nav>
```



### 4.3 实现分页和分页页码的控制

```javascript
const conn = require('../db/index.js')

// 展示首页页面
const showIndexPage = (req, res) => {
    // 每页显示3条数据
    const pagesize = 3
    const nowpage = Number(req.query.page) || 1

    // ！！！注意这里的语法，是 `` 拼接的字符串能识别 ${pagesize} 这样的，不然就得进行字符串拼接
    const sql = `SELECT blog_articles.id, blog_articles.title, blog_articles.ctime, blog_users.nickname FROM blog_articles LEFT JOIN blog_users ON blog_articles.authorId = blog_users.id ORDER BY blog_articles.id DESC limit ${(nowpage - 1) * pagesize}, ${pagesize}; SELECT count(*) as count FROM blog_articles`
    conn.query(sql, (err, result) => {
        if(err) {
                return res.render('index.ejs', {
                user: req.session.user,
                islogin: req.session.islogin,
                    article: []
            })
        }
        // 总页数
        const totalPage = Math.ceil(result[1][0].count / pagesize)
        console.log("totalPage")
        // 渲染首页页面
        res.render('index.ejs', {
            user: req.session.user,
            islogin: req.session.islogin,
            article: result[0],
            // 总页数
            totalPage: totalPage,
            // 当前展示的是第几页
            nowpage: nowpage
        })
  })
}

module.exports = {
  showIndexPage
}
```





### 4.4 完成前端功能

```html
  <!-- 分页区域 -->
  <nav aria-label="Page navigation" class="container">
      <ul class="pagination">
        <li class="<%= nowpage-1 === 0 ? 'disabled' : '' %>">
          <<%= nowpage-1 === 0 ? 'span' : 'a' %> href="?page=<%= nowpage-1 === 0 ? 1 : nowpage-1 %>" aria-label="Previous">
            <span aria-hidden="true">&laquo;</span>
          </<%= nowpage-1 === 0 ? 'span' : 'a' %>>
        </li>
        <% for(var i = 0; i < totalPage; i++){ %>
        <li class="<%= nowpage === (i+1) ? 'active' : '' %>"><a href="?page=<%= i+1 %>"><%= i+1 %></a></li>
        <% } %>
        <li class="<%= nowpage+1 > totalPage ? 'disabled' : ''  %>">
          <<%= nowpage+1 > totalPage ? 'span' : 'a'  %> href="?page=<%= nowpage+1 > totalPage ? totalPage : nowpage+1  %>" aria-label="Next">
            <span aria-hidden="true">&raquo;</span>
          </<%= nowpage+1 > totalPage ? 'span' : 'a'  %>>
        </li>
      </ul>
    </nav>
```







---



## 5.0 使用 bcrypt 加密算法

**将用户的密码以明文的形式储存在数据库中有什么问题？**

- 互联网险象丛生，数据库和后端源代码有可能被黑客以各种方式偷走。盗窃数据库数据的行为称为**拖库**，用某用户数据库的密码去其他网站继续尝试的行为叫做**撞库**

**什么是哈希算法（散列，指纹，签名）？**

常见的哈希算法有MD5,SHA-1,SHA-256等

![](.\images\1562588443059.png)

##### 

> 哈希作为名词指原始数据经过哈希算法后的结果。例：123456的哈希是xxxxx,，作为动词指将原始数据通过哈希算法进行运算。例：将123456哈希一下得到xxxxx

- **相同的明文** 哈希后的结果 **一定是相同的**
- **不同的明文** 哈希后的结果 **基本是不同的**。原因：原始数据的种类是**无限的**，32位字母和数字的组合结果是**有限的**，肯定存在有一个数据和另一个数据，经过哈希后的结果相同，这种现象称为**碰撞** 或冲突（collision）。但是要找到这个另一个数据，一般情况下，非常难。
- 数学意义上，哈希算法**不可逆**。原因：原始数据的种类是无限的，32位字母和数字的组合结果是有限的

**怎么使用哈希算法解决明文储存密码的问题？**

![1562591340040](.\images\1562591340040.png)





**用了哈希就一定安全吗**？

- 彩虹表

  将互联网上常用的密码的组合（不超过1000万）提前进行哈希计算后，把结果和原始数据一一记录，并保存下来的一张表，称为彩虹表。黑客拖库后，拿着数据库里的哈希去彩虹表里找有无此哈希，并很可能找到，再反向查看原始密码。（用一台现代台式机生成这样一张彩虹表需要几个小时）

- 碰撞

  近些年，陆续有科研学者，发现了一些方法，可以迅速找到前面提到的，能产生**碰撞**的另一个数据。

**什么是加盐 ？**

加盐就是每次哈希之前给原始数据拼接一个**随机的字符串**，这个随机的字符串就成为盐，哈希后把结果和盐都保存在数据库中，下次验证时做相同的动作





### 5.1 使用步骤



#### 步骤1：下载和导入

​	`npm i node-pre-gyp -g`

​	`cnpm install bcrypt -S`

```javascript
// 导入加密的模块
const bcrypt = require('bcrypt')
// 定义一个幂次
const saltRounds = 10
```



#### 步骤2：调用加密的方法

```javascript
// 加密的方法
bcrypt.hash('123', saltRounds, (err, pwdCryped) => {
   console.log(pwdCryped)
})
```



#### 步骤3：调用对比的方法

```javascript
// 对比 密码的方法
bcrypt.compare('123', '$2b$10$i1ufUKnC9fXTsF9oqqvLMeDnpNfYIvhyqKRG03adiebNFPkjW3HPW', function(err, res) {
  console.log(res)
  // 内部对比的过程：
  // 1. 先获取 输入的明文
  // 2. 获取输入的密文
  // 2.1 从密文中，解析出来  bcrypt 算法的 版本号
  // 2.2 从密文中，解析出来 幂次
  // 2.3 从密文中，解析出来前 22 位 这个随机盐
  // 3. compare 方法内部，调用 类似于 hash 方法 把 明文，幂次，随机盐 都传递进去     最终得到正向加密后的密文
  // 4. 根据最新得到的密文，和 compare 提供的密文进行对比，如果相等，则 返回 true ，否则返回 false;
})
```



#### 注册的处理

```javascript
// 处理注册页面的功能请求
const reg = (req, res) => {
    let body = req.body
    console.log(body)
    // 验证表单数据是否合规则
    if (body.username.trim().length <= 0 || body.password.trim().length <= 0 || body.nickname.trim().length <= 0) {
        return res.send({ msg: '请填写完整的数据', status: 501 })
    }
    // 查询用户名是否重复
    let sql1 = 'select count(*) as count from blog_users where username=?'
    conn.query(sql1, body.username, (err, result) => {
        if (err) return res.send({ 
            msg: '查重操作失败', 
            status: 502 
        })
        if (result[0].count !== 0) return res.send({ 
            msg: 'sorry，用户名重复了', 
            status: 503 
        })
        // 执行注册的业务逻辑
        body.ctime = moment().format('YYYY-MM-DD HH:mm:ss')

        // 加密处理
        bcrypt.hash(body.password, saltRounds, (err, pwd) => {
            // 如果加密失败，就注册失败
            if (err) return res.send({msg: '注册用户失败！', status: 501})
            body.password = pwd

            let sql2 = 'insert into blog_users set ?'
            conn.query(sql2, body, (err, result) => {
                if (err) return res.send({ 
                    msg: '注册操作失败！', 
                    status: 504 
                })
                if (result.affectedRows !== 1) return res.send({ 
                    msg: '注册功能失败！', 
                    status: 505 
                })
                res.send({ 
                    msg: '恭喜亲，注册用户名成功！', 
                    status: 200 
                })
            })
        })
    })
}
```



#### 登录的处理

```javascript
// 登录的请求处理函数
const login = (req, res) => {
  // 1. 获取到表单中的数据
  const body = req.body
  // 2. 执行Sql语句，查询用户是否存在
  const sql1 = 'select * from blog_users where username=?'
  conn.query(sql1, [body.username], (err, result) => {
    // 如果查询期间，执行Sql语句失败，则认为登录失败！
    if (err) return res.send({ msg: '用户登录失败', status: 501 })
    // 如果查询的结果，记录条数不为 1， 则证明查询失败
    if (result.length !== 1) return res.send({ msg: '用户登录失败', status: 502 })

    // 对比 密码的方法
    // bcrypt.compare('用户输入的密码', '数据库中记录的密码', 回调函数)
    bcrypt.compare(body.password, result[0].password, (err, compireResult) => {
      if (err) return res.send({ msg: '用户登录失败', status: 503 })

      if (!compireResult) return res.send({ msg: '用户登录失败', status: 504 })

      // 把 登录成功之后的用户信息，挂载到 session 上
      req.session.user = result[0]
      // 把 用户登录成功之后的结果，挂载到 session 上
      req.session.islogin = true
      // 查询成功
      res.send({ msg: 'ok', status: 200 })
    })
  })
}
```





---



## 6.0 发布黑马博客案例到线上服务器

> 
>
> #### 需要购买域名，域名需要备案和各种信息绑定，然后再需要租服务器，然后绑定对应的域名



​	打开cmd面板，然后输入 mstsc 

![cmd-open](images\cmd-open.jpg)



​	连接远程的服务器

![connect-net](images\connect-net.jpg)



​	打开后的界面，和普通电脑的一样

​	安装 phpstudy，启动mysql

​	安装 navicat ，录入数据

​	安装nodejs，通过 npm 给项目加载对应的包





> ## 启动了服务之后，不要关闭小黑窗口，否则项目运行不起来



## 7.0 课程阶段总结

1. nodejs、模块、ES6基础语法

   重点：this，箭头函数

2. 两个原生模块：`fs`模块，`path`模块，异步、nodejs中的模块化、模块化的标准、模块的分类、不同模块怎么引用

   重点：事件循环的三个主体概念，怎么工作的？

3. `npm`包、`http`模块

   重点：全局安装和本地安装的区别？BS架构下一次客户端获取服务器资源的完整过程

4. express，路由，静态文件托管，表单解析、模板，nodejs操作数据库

5. 模块加载顺序，express如何获取请求参数，body-Parser两种开发模式，英雄列表后端

   重点：浏览器中哪些动作会有跨域的限制？为什么要限制？JSOP的原理，CORS跨域的原理

6. 英雄列表前端、黑马博客注册登录功能

   重点：模拟前后端分离的开发流程

7. MVC，Cookie和Session，黑马博客登出功能

   重点：分层，复用，抽象等写代码的思想。Cookie和Session的概念

8. 黑马博客文章相关功能

   

   

   

   

   

1. 两个项目：(MVC架构)

   英雄列表

   - 前端：jquery + art-template + semantic-ui
   - 后端: express + mysql

   黑马博客

   - 前端：jquery + bootstrap

   - 后端:  express + ejs + mysql + (session + bcrypt)

     

