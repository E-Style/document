# NodeJS简要札记

---



## 1.0 模块加载机制

**任何模块加载前都会先去缓存查找**

① **文件**模糊查找规则：

1. 无后缀的文件
2. `.js`后缀的文件
3. `.json`后缀的文件
4. `.node`后缀的文件

② **文件夹**内搜索规则：

1. 文件夹根目录中的 `package.json`的`main`字段指定的文件
2. 文件夹根目录中以 `index` 为关键字进行文件模糊查找 ①

- `require('test')`
  - 是否是核心模块
  - 在node_modules中以 `test` 为关键字进行文件模糊查找  ①
  - 在node_modules中对 `test` 文件夹进行搜索  ②
  - 在上一级目录的node_modules中重复上面的步骤
- `require('./test.js')`
  - 是否有此文件
- `require('./test')`
  - 在指定的目录中以 `test` 为关键字进行文件模糊查找 ①
  - 在指定的目录中对 `test` 文件夹进行搜索  ②
- `require('./test/')`
  - 在指定的目录中对 `test` 文件夹进行搜索  ②





---



## 2.0 获取req参数 

​	分析不同的情况，得到的数据的方式是不一样的



#### 安装postman软件

​	用来测试请求是否发送成功，成功后获取到的相关数据有哪些

​	![postman](images\postman.jpg)



### 2.1 req.query获取查询参数

​	`npm init -y`

​	`npm i express -S`

```javascript
const express = require('express')
const app = express()

app.get('/user', (req, res) => {
    console.log(req.query)
    res.send('ok')
})

app.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000')
})
```



### 2.2 req.params获取路径参数

```javascript
const express = require('express')
const app = express()

app.get('/user/:id/:name', (req, res) => {
    console.log(req.params)
    res.send('ok')
})

app.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000')
})
```



### 2.3 req.body获取POST请求表单数据

​	`npm i body-parser -S`

```javascript
const express = require('express')
const app = express()

const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({extended: false})) //如果是true则支持嵌套的urlencoded数据

app.post('/user', (req, res) => {
    console.log(req.body)
    res.send('ok')
})

app.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000')
})
```





---



## 3.0 需求分析

后端项目运行地址：http://127.0.0.1:5000

前端项目运行地址：http://127.0.0.1:3000

### 3.1 介绍两种web开发模式

### 3.1.1 混合模式（传统开发模式）（黑马博客）

- 以后端程序员为主，基本上不需要前端程序员，或者，前端程序员只负责画页面、美化样式、写JS特效，前端程序员不需要进行数据的交互；

- 这种开发模式，在早些年比较常见；

- 传统开发模式下，用的最多的是 模板引擎  + Jquery  + Bootstrap

- 后端页面 .php   .jsp   .aspx   .cshtml

  总结：用数据填充模板并且渲染发生在**后端**

### 3.1.2 前后端分离（趋势）(英雄列表)

- 后端负责操作数据库、给前端暴露接口

- 前后端分离的好处：保证了各个岗位的职责单一；

- 前端负责调用接口，渲染页面、前端就可以使用一些流行的前端框架 Vue， React， Angular

  总结：用数据填充模板并且渲染发生在**前端**





### 3.2 项目技术栈

#### 3.2.1英雄列表

- 前端：jquery + art-template + semantic-ui
- 后端: express + mysql



#### 3.2.2黑马博客

- 前端：jquery + bootstrap
- 后端:  express + ejs + mysql

### 3.3 跨域

​	**浏览器**在给后端发送**ajax请求**的时候，如果触犯下列条件之一，就会涉及到跨域问题。

1. 协议不同
2. 域名不同
3. 端口不同

****

```javascript
$.ajax({
      url: 'https://www.zhihu.com/inbox',
      type: 'get',
      success: function(result) {
        // 如果成功拿到，测试弹个窗
        alert(result);
        // 后面再写个请求把数据发到自己服务器
      }
})

```

**跨域的限制只存在于浏览器中，脱离浏览器的环境发送请求则不存在跨域的限制**



### 3.4 JSONP和CORS的区别

1. JSONP的原理：动态创建script标签；

- JSONP发送的不是Ajax请求
- 不支持 Post 请求；

2. CORS中文意思是`跨域资源共享` ,需要服务器端进行 `CORS` 配置；

- 服务器按照`CORS`的规则进行响应后，前端发出跨域的Ajax请求响应时，就不会再被浏览器拦截，可以正常获取数据 ；

3. 对于Node来说，如果想要开启 CORS 跨域通信，只需要安装`cors`的模块即可；

### 3.5 ajax请求分为 简单请求和非简单请求

简单请求：

（1) 请求方法是以下三种方法之一：

- HEAD
- GET
- POST

（2）请求头信息不能超出以下几种字段：

- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type：只限于三个值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`

非简单请求：不是简单请求就都是非简单请求

简单请求的过程：

1. 浏览器发现请求是一个**跨域的简单请求**
2. 浏览器请求服务器
3. 服务器响应给浏览器
4. 浏览器检查：
   - 响应头里的 `Access-Control-Allow-Origin` 是否和 请求头里的 `origin`字段相同

5. 不相同则阻止 js 获取响应数据

非简单请求过程：

1. 浏览器发现请求是一个**跨域的非简单请求**
2. 浏览器先暂停这个请求，自己代劳发出一个`option`请求 （又叫预检请求，探针请求），并且把本来要发的请求方式写在请求头的`Access-Control-Request-Method`，把需要服务端额外允许的其他请求头字段名写在请求头的`Access-Control-Request-Headers`
3. 服务器响应给浏览器
4. 浏览器检查：
   - 响应头里的 `Access-Control-Allow-Origin`是否和 请求头里的 `origin`字段相同
   - 响应头里的 `Access-Control-Allow-Headers`是否和 请求头里的 `Access-Control-Request-Headers`字段相同
   - 响应头里的 `Access-Control-Allow-Methods` 是否包含 请求头里的 `Access-Control-Request-Method`字段

​    5.不相同则阻止本来要发送的请求

上述过程，浏览器还会检查响应头的`Access-Control-Allow-Credentials`字段，如果是`true`，才会在ajax请求中携带要请求的域名下的cookie，如果不通过则不会携带cookie，但不会阻止请求的顺利进行。







### 3.6 设计数据库heros表

​	id为主键自动递增

​	name 字符类型，不为空

​	gender 字符类型，不为空

​	ctime 字符类型，不为空

​	isdel  tinyint类型，默认值为0



---



## 4.0 设计接口

​	创建 API_server 文件夹，下载 express 模块，运行在5001端口



### 4.1 创建后端API项目基本结构

```javascript
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.send('后台数据接口访问成功')
})

app.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000')
})
```



### 4.2 获取所有英雄接口

​	`npm i mysql -S`

```javascript
// 导入数据库模块，创建数据库连接
const mysql = require('mysql')
const conn = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'node_item'
})

// 设计获取请求所有的英雄接口
app.get('/getAllHeros', (req, res) => {
    // 4.1 编辑sql语句
    let sqlStr = 'SELECT * FROM heros'
    conn.query(sqlStr, (err, result) => {
        if (err) res.send({ 
            status: 500, 
            msg: err.message, 
            data: null })
        // 4.2 返回请求的数据
        res.send({ 
            status: 200, 
            msg: 'ok', 
            data: result 
        })
    })
})
```



### 4.3 添加英雄的接口

```javascript
// 导入解析表单post提交时传输的数据
const bodyParser = require('body-parser')
// 使用中间件
app.use(bodyParser.urlencoded({ extended: false }))

// 设计添加英雄的接口，获取表单post提交时的数据并操作到数据库中
app.post('/addheros', (req, res) => {
    let hero = req.body
    // 创建时间并格式化数据，添加在req.body中
    let dt = new Date()
    let y = (dt.getFullYear() + '').padStart(2, '0')
    let m = (dt.getMonth() + 1).toString().padStart(2, '0')
    let d = dt.getDate().toString().padStart(2, '0')
    let hh = dt.getHours().toString().padStart(2, '0')
    let mm = dt.getMinutes().toString().padStart(2, '0')
    let ss = dt.getSeconds().toString().padStart(2, '0')
    hero.ctime = y + '-' + m + '-' + d + ' ' + hh + ':' + mm + ':' + ss
    // 编辑sql语句并执行
    let sql = 'insert into heros set ?'
    conn.query(sql, hero, (err, result) => {
        if (err) res.send({
            status: 500,
            msg: err.message,
            data: null
        })

        res.send({
            status: 200,
            msg: 'ok',
            data: null
        })
    })

})
```



### 4.4 根据id查询英雄的接口



```javascript
// 根据id获取英雄的信息
app.get('/gethero/:id', (req, res) => {
    let id = req.params.id
    let sql = 'SELECT * FROM heros WHERE id = ?'
    conn.query(sql, id, (err, result) => {
        if (err) res.send({ 
            status: 500, 
            msg: err.message, 
            data: null })
        // 4.2 返回请求的数据
        res.send({ 
            status: 200, 
            msg: 'ok', 
            data: result 
        })
    })
})
```





### 4.5 根据id更新英雄的接口

```javascript
// 根据id更新英雄的信息
router.post('/updatehero/:id', (req, res) => {
        let id = req.params.id
        let newInfo = req.body
        let sql = 'update heros set ? where id = ?'
        conn.query(sql, [newInfo, id], (err, result) => {
            if (err) res.send({
                status: 500,
                msg: err.message,
                data: null
            })
            res.send({
                status: 200,
                msg: 'ok',
                data: {
                    affectedRows: result.affectedRows
                }
            })
        })
    }
```





### 4.6 根据id软删除英雄的接口



```javascript
// 根据id对英雄进行软删除
router.get('/deletehero/:id', (req, res) => {
        let id = req.params.id
        let sql = 'update heros set isdel = 1 where id = ?'
        conn.query(sql, id, (err, result) => {
            if (err) res.send({
                status: 500,
                msg: err.message,
                data: null
            })
            res.send({
                status: 200,
                msg: 'ok',
                data: {
                    affectedRows: result.affectedRows
                }
            })
        })
    })
```





## 5.0 封装API_SERVER项目



### 5.1 服务模块

​	server.js

```javascript
// 导入express模块创建服务器
const express = require('express')
const app = express()
// 导入解析表单post提交时传输的数据
const bodyParser = require('body-parser')
// 使用中间件
app.use(bodyParser.urlencoded({ extended: false }))

// 导入 router 模块
const router = require('./router.js')
// 使用app.use注册功能
app.use(router)

// 启动服务器，运行在5001端口
app.listen(5001, () => {
    console.log('server running at http://127.0.0.1:5001')
})
```



### 5.2 路由模块

```javascript
// 导入express模块，创建路由对象
const express = require('express')
const router = express.Router()
// 引入函数集模块
const ctls = require('./functions.js')

// 监听客户端根目录请求，返回基本数据
router.get('/', ctls.testFn)
// 设计获取请求所有的英雄接口
router.get('/getallheros', ctls.getAllHeros)
// 设计添加英雄的接口，获取表单post提交时的数据并操作到数据库中
router.post('/addheros', ctls.addHero)
// 根据id获取英雄的信息
router.get('/gethero/:id', ctls.getHeroById)
// 根据id更新英雄的信息
router.post('/updatehero/:id', ctls.updateById)
// 根据id对英雄进行软删除
router.get('/deletehero/:id', ctls.deleteById)

// 向外暴露路由对象
module.exports = router
```





### 5.3 函数集

```javascript
const conn = require('./db.js')

// 直接暴露出去处理任务的函数
module.exports = {
    testFn: (req, res) => {
        res.send('请求后台接口成功~')
    },
    getAllHeros: (req, res) => {
        // 4.1 编辑sql语句
        let sqlStr = 'SELECT * FROM heros'
        conn.query(sqlStr, (err, result) => {
            if (err) res.send({ 
                status: 500, 
                msg: err.message, 
                data: null })
            // 4.2 返回请求的数据
            res.send({ 
                status: 200, 
                msg: 'ok', 
                data: result 
            })
        })
    },
    addHero: (req, res) => {
        let hero = req.body
        // 创建时间并格式化数据，添加在req.body中
        let dt = new Date()
        let y = (dt.getFullYear() + '').padStart(2, '0')
        let m = (dt.getMonth() + 1).toString().padStart(2, '0')
        let d = dt.getDate().toString().padStart(2, '0')
        let hh = dt.getHours().toString().padStart(2, '0')
        let mm = dt.getMinutes().toString().padStart(2, '0')
        let ss = dt.getSeconds().toString().padStart(2, '0')
        hero.ctime = y + '-' + m + '-' + d + ' ' + hh + ':' + mm + ':' + ss
        // 编辑sql语句并执行
        let sql = 'insert into heros set ?'
        conn.query(sql, hero, (err, result) => {
            if (err) res.send({
                status: 500,
                msg: err.message,
                data: null
            })
    
            res.send({
                status: 200,
                msg: 'ok',
                data: null
            })
        })
    
    },
    getHeroById: (req, res) => {
        let id = req.params.id
        let sql = 'SELECT * FROM heros WHERE id = ?'
        conn.query(sql, id, (err, result) => {
            if (err) res.send({ 
                status: 500, 
                msg: err.message, 
                data: null })
            // 4.2 返回请求的数据
            res.send({ 
                status: 200, 
                msg: 'ok', 
                data: result 
            })
        })
    },
    updateById: (req, res) => {
        let id = req.params.id
        let newInfo = req.body
        let sql = 'update heros set ? where id = ?'
        conn.query(sql, [newInfo, id], (err, result) => {
            if (err) res.send({
                status: 500,
                msg: err.message,
                data: null
            })
            res.send({
                status: 200,
                msg: 'ok',
                data: {
                    affectedRows: result.affectedRows
                }
            })
        })
    },
    deleteById: (req, res) => {
        let id = req.params.id
        let sql = 'update heros set isdel = 1 where id = ?'
        conn.query(sql, id, (err, result) => {
            if (err) res.send({
                status: 500,
                msg: err.message,
                data: null
            })
            res.send({
                status: 200,
                msg: 'ok',
                data: {
                    affectedRows: result.affectedRows
                }
            })
        })
    }
}
```



### 5.4 数据库连接模块

```javascript
// 导入数据库模块，创建数据库连接
const mysql = require('mysql')
const conn = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'node_item'
})

module.exports = conn
```





---



## 6.0 安装semantic-ui



### 6.1 全局安装Gulp

​	`npm i gulp -g`

![downGulp](images\downGulp.jpg)



### 6.2 初始化项目

​	`npm init -y`

![web-init](images\web-init.jpg)



### 6.3 下载semantic-ui

​	`npm i semantic-ui -S`

![autosemantic](images\autosemantic.jpg)



### 6.4 进程操作

​	![semantic-yes](images\semantic-yes.jpg)



![semantic-ok](images\semantic-ok.jpg)



![semantic-all](images\semantic-all.jpg)



### 6.5 使用gulp工具完成操作

​	`cd semantic`

​	`gulp build`

![semantic-gulp](images\semantic-gulp.jpg)

