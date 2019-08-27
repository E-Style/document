# PHP\mySQL\Ajax整理

---

## PHP相关

### 1.0 网络相关知识

服务器：远程计算机，用来存储项目，需要部署软件和关于前后端及数据库的一些服务

ip地址：每一个服务器都会有一个绝对的地址，由4个0-255的号段组成

端口：默认端口为80，https为443，数据库为3306

域名：需要购买，方便记忆，需要通过DNS服务商进行解析成对应IP地址

url：一段完整的包含协议、域名、端口、文件和键值对参数的网址



### 2.0 使用PHPstudy

先安装再使用，默认安装在C盘，不要有中文路径

phpstudy文件夹中PHPTutorial文件夹内的WWW文件夹就是web容器，项目文件需要放在这里

使用127.0.0.1访问文件



### 3.0 php基本语法

##### 3.1 输出语句

| 输出语句        | 说明            |
| ----------- | ------------- |
| echo        | 关键字，输出字符串     |
| print( )    | 方法，输出一个字符串    |
| print_r( )  | 输出复杂数据类型      |
| var_dump( ) | 输出复杂数据类型的具体信息 |

##### 3.2 混编

```php
<?php
  	if ($id > 5) {
?>
  	<li>就输出这里的值</li>
<?php
  	}
?>
```

##### 3.3 变量和注释

| 方法       | 功能                                       |
| -------- | ---------------------------------------- |
| isset( ) | 判断当前变量是否存在，判断变量是否定义了，判断当前的值是否为null       |
| empty( ) | 判断变量是否为空值 -   ""  0  "0"  null  false  array( ) |
| unset( ) | 删除变量，可以删除多个，用逗号隔开                        |

```php
// line common
/*
	block common
*/
```

##### 4.0 数据类型

| 数据类型    | 解释                |
| ------- | ----------------- |
| string  | 字符串               |
| integer | 整型 - 只能是整数        |
| float   | 浮点型 - 小数          |
| boolean | 布尔型 - true或者false |
| array   | 数组                |
| object  | 对象                |
| NULL    | 空                 |

| 基本数据类型                       | 复合数据类型       | 特殊类型      |
| ---------------------------- | ------------ | --------- |
| string    字符串                | array    数组  | NULL    空 |
| integer    整型 - 只能是整数        | object    对象 | 资源        |
| float    浮点型 - 小数            |              |           |
| boolean    布尔型 - true或者false |              |           |

| 方法名称         | 功能             |
| ------------ | -------------- |
| is_string( ) | 判断当前变量是否为字符串类型 |
| is_bool( )   | 判断当前变量是否是布尔类型  |
| is_int( )    | 判断是否是整型        |
| is_float( )  | 判断是否是浮点型       |
| is_array( )  | 判断是否为数组类型      |
| is_object( ) | 判断当前变量是否是对象类型  |

```php
// ''不解析变量
// ""解析变量，最好加一组{}
```



### 4.0 数组的操作

##### 4.1 索引数组

```php
<?php
  	array(1, 2, 3, true, "abc");
	// 循环数组
    for($i = 0; $i < count($arr); $i++) {
      	echo $arr[$i]." "; // 使内容之间有些距离
	}
?>
```

##### 4.2 关联数组

```php
<?php
  	array (
  		"name" => "yiyang",
  		"age" => 20,
  		"gender" => true
	);
	// 循环数组
	foreach($arr as $value) {
      echo $value . "</br>";
	}
?>
```

##### 4.3 二维数组

```php
<?php
  	array(
  		array(
        	"name" => "tylor",
          	"age" => 20
        ),
  		array(
        	"name" => "Bluce",
          	"age" => 18
        )
	);
?>
```

​	__计算数组的长度用count()，删除数组的某一项用unset()__



### 5.0 数据类型转换和运算符

| 数据类型   | 默认值   |
| ------ | ----- |
| int    | 0     |
| object | null  |
| bool   | false |
| float  |       |

| 算术运算符              | 赋值运算符        | 逻辑运算符        | 比较运算符             |
| ------------------ | ------------ | ------------ | ----------------- |
| + 加 -  减  * 乘  / 除 | = 一般赋值       | ！取反          | > 大于 < 小于         |
| % 求余，相除完剩余的结果      | += 在本身追加     | &&  与，严格要求   | >= 大于等于  <=小于等于   |
| ++  自加单独没区别        | -= 在本身扣除     | \|\|  或，满足一项 | == 是否相等  === 是否全等 |
| -- 同上              | *=  /=  同上逻辑 |              | != 不相等   !==  不全等 |

**三元运算符**        表达式会返回true或者false      **?**        如果为true返回值1      **:**      如果为false返回值2

__打印true为1，打印false为空白__



### 6.0 php常用语法

##### 6.1 if语句、循环、函数

```php
<?php
  	$age = 40;
	if($age > 18) {
      	echo '您已成年';
	} else {
      	echo  '您未成年';
    }
	
	for($i = 0; $i < 10; $i++) {
      	// 输出语句
  	}

	// 函数声明
    function getSum($num) {
      $sum = 0;
      for($i = 0; $i <= $num; $i++) {
          $sum += $i
      }
      return $sum;
    }
    echo getSum(100);
?>
```

##### 6.2 超全局变量

| 超全局变量名称   | 作用              |
| --------- | --------------- |
| $GLOBALS  | 引用全局作用域中可用的全部变量 |
| $_SERVER  | 获取服务端相关信息       |
| $_REQUEST | 获取提交参数          |
| $_POST    | 获取post提交参数      |
| $_GET     | 获取get提交参数       |
| $_FILES   | 获取上传文件          |
| $_ENV     | 操作环境变量          |
| $_COOKIE  | 操作cookie        |
| $_SESSION | 操作session       |

##### 6.3 常量

定义

```php
<?php
  	// ！！！定义说明  不可修改，区分大小写，一般用大写
  	// define(name, value, insensitive); insensitive -> 不敏感，迟钝的
  	define("PI", 3.14, false); // 设置为true则代表不区分大小写

	echo PI; // 3.14
?>
```

| 常量名称          | 作用                   |
| ------------- | -------------------- |
| \__LINE__     | 可以获取当前的代码行           |
| \__FILE__     | 可以获取当前文件的路径   目录+文件名 |
| \__DIR__      | 可以获取当前文件的目录          |
| \__FUNCTION__ | 可以获取当前魔术常量所在的函数      |

##### 6.4 文件载入关键字

```php
<?php
  	// include 相当于在这里复制粘贴了一份
  	include 'constant.php'; 
	echo SCHOOL_NAME; // 传智播客

  	// include_once   只会载入一次
  	include_once 'constant.php'; // 
	echo SCHOOL_NAME; // 传智播客 

  	// require  
  	require 'constant.php'; 
	echo SCHOOL_NAME; // 传智播客

  	// require_once
	require_once 'constant.php'; 
  	echo SCHOOL_NAME; // 传智播客
?>
```

##### 6.5 常用api

​	获取字符串长度

| 方法                      | 作用                                 |
| ----------------------- | ---------------------------------- |
| strlen( )               | 获取字符串的长度（中文3个字节）                   |
| mb_strlen( )            | 没有设置编码，就使用php默认的编码（中文1个字节，5.6版本以上） |
| mb_internal_encoding( ) | 获取php内部默认的编码                       |

​	获取时间

| 方法           | 名词      | 释义                                       |
| ------------ | ------- | ---------------------------------------- |
| time( )      | 时间戳     | 从Unix纪元(格林威治 1970-01-01 00:00:00)到现在的秒数  |
| date( )      | 格式化日期   | date("Y-m-d H:i:s")   或者   date("Y-m-d H-i-s",  $time) |
| strtotime( ) | 转换时间为秒数 | 把日期格式转换为秒数的时间戳                           |

​	__需要修改配置文件的时区__

##### 6.6 文件操作

| 方法                   | 功能       | PHP  |
| -------------------- | -------- | ---- |
| file_get_contents( ) | 将文件读入字符串 | 4    |
| file_put_contents( ) | 将文件写入字符串 | 5    |

```php
<?php
  	// header("Content-Type:image/jpg");  读取图片要加
	$res1 = file_get_contents("./images/monkey.png");
	echo $res1;
  
	// 需要加入一个参数 FILE_APPEND
	file_put_contents("data.txt", "这是我写入的内容", FILE_APPEND);
?>
```



### 7.0 GET请求和POST请求

​	在form的action中或者a链接的href属性中写入地址，form里面可以设置post方式，其他的不可以

```php
<?php
  // !!! 判断当前用户是否发送了请求，	是否是以post方式
  // print_r($_SERVER); 很大一串关联数组， REQUEST_METHOD
  if($_SERVER['REQUEST_METHOD'] === 'POST') {
    	echo '我的名称是'.$_POST['userName123'].', 我的密码是'.$_POST['userPwd'];
  }
?>
```

收集数据说明

```php
<?php
  	print_r($_GET); // 不管在怎么选择, 但是gender始终是on
	// Array([userName] => jack [userPwd] => 123 [gender] => on)

	/*	==> 单选框
		系统会自动的收集当前表单元素的value值
		解决办法: 给单选按钮添加value值，区分一下
	*/

	/*  ==> 复选框
		如果复选框的名称一样，那么就会默认传递最后一个选项的值
		如果需要传入所有选中的复选框的数据，可以在name属性值后面添加[]
		添加了[]， 在收集数据的时候，就会把他们放进一个数组内
		eg:
		爱好：<input type="checkbox" name="hobby[]"  value="code"> 写代码 
      		  <input type="checkbox" name="hobby[]"  value="reading"> 看书  <br>
	*/
?>
```

​	__通过get提交的就用$_GET获取数据，__

​	__通过post提交就用$POST获取__

​	__通过post上传文件，就用$_FILES获取__

##### 7.1 上传文件

​	提交请求

```php
// !!! 在php中文件上传必须得是post请求
/*
	1, 必须给表单设置enctype属性
		application/x-www-form-unlencoded    将参数编码为键值对的格式，标准格式
			(UTF-8 GBK GB2312)  用来处理字符串，默认的编码格式 
		multipart/form-data   专门用来处理特殊文件，比如文件
*/
<form action="upload.php" method="post" enctype="multipart/form-data">
  	文件选择： <input type="file" name="myFile"> <br>
  	<input type="submit">
</form>
```

​	上传操作

```php
<?php
  	// 在php中，上传之后的相关信息都存储在 $_FILES 超全局变量中
  	// 文件夹是手动创建的
  	move_uploaded_file($_FILES["myFile"]["tmp_name"], "./upload/temp.png")
?>
```

| 方法                    | 作用                  | 示例                               |
| --------------------- | ------------------- | -------------------------------- |
| move_uploaded_file( ) | 移动文件                | move_uploaded_file(文件存储位置, "路径") |
| strpos(源字符, str)      | 搜索字符在源字符第1次出现的索引，数值 | strpos($type, "image/")          |
| strrchr(aa, ".")      | 获取当前符号右侧所有的字符串      | strrchr($fileName, ".")          |
| rand(1000, 9999)      | 获取随机数               |                                  |

__注意文件不要取中文名，注意文件大小有限制，需要修改配置文件重启服务器__



### 8.0 HTTP协议

HTTP协议：HTTP 是基于 TCP/IP 协议的应用层协议。它不涉及数据包（packet）传输，主要规定了客户端和服务器之间的通信格式

 TCP/IP 协议建立与服务器连接有三次握手

##### 8.1 请求报文

请求行   请求头  请求体

get请求没有请求体

post请求一定要有请求头

##### 8.2 响应报文

状态行   响应行  响应体

最终返回给浏览器端的数据都在响应体内

##### 8.3 使用header方法

```php
<?php
  	// 设置响应头的类型
  	header("Content-Type:text/html;charset=utf-8;");
  	
	// 立马做出跳转
  	// header("Location:01-getsmt.php");
  
  	// 在指定的时间之后跳转
  	header("refresh:3;url=01-getsmt.php");

	// 实现当前页面的自动下载
  	header("Content-Type:application/octet-stream");

	// 指定文件名称, 自动下载，设置下载名称
	header("Content-Disposition:attachment;filename=tmp.php");
?>
```



### 9.0 数据转存

##### 9.1 http协议无状态

​	因为HTTP协议是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了交互式Web应用程序的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两瓶饮料。最后结帐时，由于HTTP的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么，所以Cookie就是用来绕开HTTP的无状态性的“额外手段”之一

##### 9.2 cookie  客户端，不安全

```php
<?php
  	// 创建cookie
  	// setcookie("username", "tylor");  // 可以在请求头中查看
  
	// 判断是否拥有某个指定名称的cookie值 -- $_COOKIE
	if(isset($_COOKIE["username"])) {
      	echo "欢迎回来，朕的小仙女";
	}else {
		echo "大人头一回来，是打尖儿还是住店呀~";
      	setcookie("username", "tylor");
    }
	
	setcookie("username", "tylor", time() + 10);
	// 通过path可以设置访问权限，参照网站根目录
	setcookie("username", "tylor", PHP_INT_MAX, "/day05/down");
	// 设置父级目录，子目录可以访问，设置子目录，上层不能访问  "/" 代表整站可以访问
?>
```

9.3 session 服务器端，安全

session 是通过 cookie来工作的

```php
<?php
  	// !!! php中默认不能使用session功能，如果需要使用则需要手动设置
	session_start();
	/*
		1, 在服务器端动态生成一个sessionID
		2, 在服务器端动态生成一个可以存放本次会话数据的文件，文件名以sess_sessionID构成
		3, 通过相应头动态设置cookie， 在cookie中存放了本次会话所生成的sessionID
	*/
  
  	// 创建session  使用超全局变量 $_SESSION["name"] = value;
  	$_SESSION["user"] = Array(
							"name" => "dilireba",
  							"age" => 25
						);
?>
```



---





## mySQL 数据库

### 1.0 navicat的使用

​	打开服务器，打开数据库，新建连接，密码root，新建数据库

​	设置表头，设置主键和递增，设置默认值要加引号，设置非null

​	点击查询，新建查询，执行sql语句

### 2.0 增删改查(CURD)

```sql
-- 查找
select * from mytable where age < 20 and gender = 0

-- 增加
insert into mytable(name, age, gender) values('lili', 30, 0)

-- 修改
update mytable set age = age + 1,gender = 1 where id = 5

-- 删除
delete from mytable where id = 8
```

##### 2.1 几个功能

```sql
-- 选择符合条件的记录数
select count(id) from mytable

-- max 获取最大值  min  获取最小值
select max(age) from mytable

-- 一般都是数值
select svg(age) from mytable

-- 实现按照性别，再按照年龄
select * from mytable order by gender,age

-- 中间范围的记录   n 偏移量从0开始, m 能够获取的记录数
select * from mytable limit 2,2

-- 1.0 采用from where的方式
select * from student,class where student.cid = class.classid  -- where后面的这个 = 表示判断

-- 2.0  join 和 inner join都是一样的   on和where的意思也是差不多的
select * from student inner join class on student.cid = class.classid
```

##### 2.2 php连接数据库

```php
<?php
  	header("Content-Type:text/html;chartset=utf-8");
  	// 为了保证服务器编码，与当前php编码保持一致，可以设置服务器返回数据的编码
  	mysqli_set_chartset($conn, "utf8");

  	// 这个函数会自动打开连接
  	$conn = mysqli_connect("localhost", "root", "root", "mybase");
	// 假如出错了，错误信息会给详细的提示
	if(!$conn) {
      	die("连接失败");
	} else {
      	echo "连接成功";
	}
	echo "这是连接之后的代码，如果有错误，将不会执行到这里";
?>
```

##### 2.3 执行sql语句

```php
<?php
  	// 新增数据 创建sql语句
  	$sql = "insert into mytable value(null, '张三', 30, 1)";
	// 执行sql语句  mysqli_query(连接对象， sql语句)
	$result = mysqli_query($conn, sql);
	if($result) {
      	echo "新增成功";
	} else {
      	// 输出具体的报错信息
      	echo mysqli_error($conn); 
	}
?>
```

##### 2.4 展示查询的结果

```php
<?php
  	// ... 上面的连接和判断的代码省略
  	$sql = "select * from mytable";
	$result = mysqli_query($conn, $sql);
	if(!$result) {
      	die("查询失败");
	} else if(mysqli_num_rows($result) == 0) {
      	die("结果集为空");
	} else {
      	var_dump($result);
	}
  	// ！！！！循环并不知道何时停止，while循环就比较适合
  	while ($arr = mysqli_fetch_array($result, MYSQL_ASSOC);) {
      	$res[] = $arr;
  	}
	print_r($res);
?>
```

| 方法                                       | 说明                |
| ---------------------------------------- | ----------------- |
| mysqli_connect("主机名称", "用户名"， “密码”， “数据库名”) | php连接MYSQL数据库     |
| mysqli_set_charset(连接对象，“utf8”);         | 设置数据库和服务器编码相同     |
| mysqli_query($conn, "set name as utf-8"); | 设置数据库和服务器编码的另一种方式 |
| mysqli_query(连接对象，sql语句)                 | 执行sql语句           |
| mysqli_num_rows(结果集[执行完sql语句之后的值])       | 返回结果集的条数          |
| mysqli_fetch_array(结果集，MYSQL_ASSOC)      | 提取查询语句之后的内容，需要循环  |
| mysqli_fetch_assoc(结果集)                  | 提取到查询语句之后的关联数组    |
| mysqli_fetch_row(结果集)                    | 提取到查询语句之后的索引数组    |
| mysqli_error(连接对象)                       | 返回具体的连接错误信息       |
| die(”提示错误信息“)                            | 终止操作，提示错误信息       |



### 3.0 php操作数据库的封装

```php
<?php
  	header("Content-type:text/html;charset=utf-8");
    // 1.0  封装增加、删除和修改的操作
    function opt($sql) {
  		// 创建连接
    	$conn = mysqli_connect("localhost", "root", "root", "mytable");
  		// 如果连接成功返回连接对象，如果不成功，返回false
  		if(!$conn) {
          	die("数据库连接失败~");
  		}
  		// 设置编码
  		mysqli_set_chartset($conn, "utf8");
  		// 执行$sql语句，接收返回值
  		$res = mysqli_query($conn, $sql);
  		// 及时关闭连接
  		mysqli_close($conn);
  		// 返回结果
  		return $res;
    }
    // 2.0  封装查询操作
  	function select() {
      	// 创建连接
    	$conn = mysqli_connect("localhost", "root", "root", "mytable");
  		// 如果连接成功返回连接对象，如果不成功，返回false
  		if(!$conn) {
          	die("数据库连接失败~");
  		}
  		// 设置编码
  		mysqli_set_chartset($conn, "utf8");
  		// 查询语句的返回值，成功返回资源（结果集），不成功返回false
  		$res = mysqli_query($conn, $sql);
      	if(!$res) {
          	echo "查询失败";
      	} else if (mysqli_num_rows($res) == 0) {
          	echo "没有查找到数据~";
      	} else {
          	// 有结果集也有数据行
          	while($arr = mysqli_fetch_assoc($res)) {
              	$result = $arr;
          	}
          	// 及时关闭连接
  			mysqli_close($conn);
          	return $return;
      	}
      
      	// 操作完毕，关闭连接
  		mysqli_close($conn);
  	}
?>
```

使用

```php
<?php
  	header("Content-type:text/html;charset=utf-8");
  	// 载入封装的文件
  	include_once "./common.php";
	$res = opt("insert into mytable values(null, '张三', 28, '男', 1112334)");
	// 如果有限制唯一的值，注意在写入的时候会报错
	if($res) {
      	echo "新增成功";
	} else {
      	echo "新增失败";
	}
?>
```





---



## Ajax 

#### 1.0 异步和请求

异步：在发起请求之后，不用理会这个过程，等请求结束了，自动返回回来数据，不用进行等待；

##### 1.1 get请求

```javascript
// 1.1 创建异步对象
var xhr = new XMLHttpRequest();
// 1.2 请求行发送 open(请求方式，请求url+键值对的参数):
xhr.open("get","validate.php?username="+name); // url地址 ? 键=值 & 键=值 & 键=值
// 1.3 get请求没有请求头
// 1.4 请求体:发送请求 
xhr.send(null);

// 1.5 通过readystate发生改变的事件处理响应
xhr.onreadystatechange = function(){
    // 1.6 判断请求是否结束后成功返回   判断服务器状态码是否为200
    if(xhr.readyState == 4 && xhr.status == 200){
        // 通过xhr对象的responseText接收返回的数据，写入盒子内
        document.querySelector(".showmsg").innerHTML = xhr.responseText;
    }
}
```

##### 1.2 post请求

```javascript
// 1.1 创建异步对象
var xhr = new XMLHttpRequest();
// 1.2 设置请求行 open(请求方式，请求url)
// post请求不需要拼接参数
xhr.open("post","validate.php");
// 1.3 设置请求头:setRequestHeader()
xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
// 1.4 设置请求体 send()
// post的参数在这个函数中设置(如果有参数)
xhr.send("username="+name);

// 1.5 通过readystate发生改变的事件处理响应
xhr.onreadystatechange = function(){
    // 1.6 判断请求是否结束后成功返回   判断服务器状态码是否为200
    if(xhr.readyState == 4 && xhr.status == 200){
        // 通过xhr对象的responseText接收返回的数据，写入盒子内
        document.querySelector(".showmsg").innerHTML = xhr.responseText;
    }
}
```



![kuayu](07a_ajax那点事儿\images\ajax.jpg)



#### 2.0 JSON文件

| 关于json的描述                       |
| ------------------------------- |
| 1，一组花括号表示一个对象，一个对象通过键值对写入一堆相关数据 |
| 2，一组方括号表示一个数组，多组对象通过数组的方式装载     |
| 3，对象的所有属性都必须加上双引号，值没有undefined  |
| 4，文件后缀名为.json，json格式的数据内不允许写注释  |

```json
[
    {
       "src":"./images/nav_1.png" ,
       "text":"京东超市"
    },
    {
        "src":"./images/nav_2.png" ,
        "text":"全球购物"
     },
     {
        "src":"./images/nav_3.png" ,
        "text":"京东市场"
     }
]
```

| 前端操作json的方式            |                         |
| ---------------------- | ----------------------- |
| JSON.parse(json字符串)    | 将json格式的字符串转换为数组或者对象    |
| JSON.stringify(对象或者数组) | 将字面量对象或者数组转换为json格式的字符串 |

| php操作json的方式         |                          |
| -------------------- | ------------------------ |
| json_decode(json字符串) | 将json格式的字符串转换为php的数组或者对象 |
| json_encode(关联数组)    | 将php的数组转换为json字符串        |

#### 3.0 封装ajax请求

```javascript
var dilireba = {
    // 将{"name":"jack","age":20} 的参数要转换为 ?name=jack&age=20
    getpa:function(data){
        if(data && typeof data == "object"){
            var str = "?";
            for(var key in data){
                str = str + key + "=" + data[key] + "&";
            }
            str = str.substr(0,str.length-1);
        }
        return str;
    },
    ajax:function(option){
        // 接收用户参数进行相应处理
        var type = option.type || 'get';
        // location.href 可以获取当前文件的路径 
        var url = option.url || location.href;
        // 接收参数:在js中最方便收集的数据类型为对象，所以我们就规定传递的参数必须是对象
        var data = this.getpa(option.data) || "";
        // 响应成功之后的回调函数 => 这个函数一般就是处理字符拼接，标签渲染的函数
        var success = option.success;
        
      	// 创建异步对象
        var xhr = new XMLHttpRequest();
        // 请求行  如果是get请求就需要拼接参数
        if(type == "get"){
            url += data;
            data = null; // 拼接后设置data为null，在send()方法中就不会再次发送
        }
        xhr.open(type,url);
        // 请求头  如果是post才需要请求头
        if(type == "post"){
            xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
        }
        // 请求体
        xhr.send(data);
        // 让异步对象接收响应
        xhr.onreadystatechange = function(){
            // 一个成功的响应有两个条件：1.服务器成功响应 2.数据解析完毕可以使用
            if(xhr.status == 200 && xhr.readyState == 4){
                // 接收响应的返回值 responseText   responseXML
                var rh = xhr.getResponseHeader("Content-Type");
                if(rh.indexOf("xml") != -1){
                    var result = xhr.responseXML;
                }
                else if(rh.lastIndexOf("json") != -1){
                    var result = JSON.parse(xhr.responseText);
                }else{
                    var result = xhr.responseText;
                }
                // 接收数据之后，调用回调函数
                success && success(result)
            }
        }
    },
}
```



#### 4.0 jQuery中ajax的使用

```javascript
$.ajax({
    type: 'post',
    url: './server/nav-json.php',
    data: {},
    success: function (result){
        var data = JSON.parse(result);
        var arr = [];
        for(var i=0;i<data.length;i++){
        	var str = "<li>"
            		+ '<a href="#">'
            		+ '<img src="' + data[i].src + '" alt="">'
            		+ '<p>' + data[i].text + '</p>'
            		+ '</a>'
            		+ '</li>';
            arr.push(str)    
		}
        // 将生成的页面结构添加到dom元素中
		document.querySelector("ul").innerHTML = arr.join("");
    }
});
```

![kuayu](07a_ajax那点事儿\images\jquery-ajax.jpg)



#### 5.0 模板引擎的使用

步骤1：

```html
<script src="./js/template-native.js"></script>
```

步骤2：

```html
<script type="text/template" id="navTemp">
	<li>
  		<a href="#">
  			<img src="<%= src %>" alt="">
  			<p><%= text %></p>
  		</a>
  	</li>
</script>
```

步骤3：

```html
<script>
	var obj = {
      	"src": "./images/nav-1.png",
      	"text": "京东超市"
	}
    // 调用函数  templete(模板id, 数据(对象))  返回替换后的DOM结构
    var html = template(navTemp, obj);
  	document.querySelector("ul").innerHTML = html;
</script>
```

##### 5.1 简洁语法

引入简洁文件，书写简洁语法，调用方法还是一样

```html
<script type="text/template" id="musicTemp">
	{{ each items as value index }}
	<tr>
		<td>{{ items[index].title }}</td>
		<td>{{ value.singer }}</td>
		<td>{{ value.album }}</td>
		<td>
  			<audio src="{{ value.src }}" controls></audio>
  		</td>
  		<td>
  			<a href="./edit.php?id={{ value.id }}" class="btn btn-primary">编辑</a>
  			<a href="./edit.php?id={{ value.id }}" class="btn btn-danger">删除</a>
  		</td>
	</tr>
	{{ /each }}
</script>
```

总结调用（参数2必须为对象）:

​	1，如果接收的是对象，那么先打印对象看看对应的数组叫什么，在模板里面就循环什么

​	2，如果接收的是数组，那么先包装成对象，给数组取什么名字，在模板里面就循环什么



#### 6.0 跨域

##### 6.1 服务器端设置跨域

```php
<?php
	// 设置跨域请求
	header("Access-Control-Allow-Origin:*"); // * 允许代表所有域来请求
	header("Access-Control-Allow-Origin:http://day09.com");

	echo file_get_contens("nav.json");
?>
```

##### 6.2 前端使用jsonp跨域

```javascript
$.ajax({
  	type: "get",
  	url: "http://day8.com/getnav.php",
  	dataType: "jsonp",
  	success: function(res) {
      	var html = template("navTemp", {"items": res})
        document.querySelector("ul").innerHTML = html
  	}
})
```

##### 6.3 跨域的原理

![kuayu](a_ajax基础\images\jsonp-in.jpg)



#### 7.0 XMLHttpRequest2.0

##### 7.1 timeout

```javascript
document.querySelector("button").onclick = function(){
	var xhr = new XMLHttpRequest();

    // 设置请求行
    xhr.open("get","01-timeout.php");
    // 设置请求头:get不需要设置
    // 设置请求体
    xhr.send(null);

    // 设置超时
    xhr.timeout = 2000;
    xhr.ontimeout = function(e){
       console.log(e);
    }

    // 接收响应
    xhr.onreadystatechange = function(){
         if(xhr.status == 200 && xhr.readyState == 4){
              alert(xhr.responseText);
          }
    }
}
```

##### 7.2 FormData

```html
<script>
        document.querySelector("#sub").onclick = function(){
            var xhr = new XMLHttpRequest();

            xhr.open("post","02-formData.php");
            // 1.手动拼接
            // 2.如果是jq,那么就可以使用表单序列化方法
            // 3.现在在XMLHttpRequest2.0   ,我们可以使用FormData来收集表单数据

            // 1.获取表单
            var myform = document.querySelector("#form1");
            // 2.将表单做为参数传递，在创建formData对象的时候
            var formdata = new FormData(myform);

            // 特点之一：可以自由的追加参数
            formdata.append("address","传智播客");
            // 3.生成的formData对象就可以直接做为异步对象的参数传递
            xhr.send(formdata);

            xhr.onreadystatechange = function(){
                if(xhr.status == 200 && xhr.readyState == 4){
                    console.log(xhr.responseText);
                }
            }
        }
</script>
```

##### 7.3 上传文件

```html
<script>
        document.querySelector("#sub").onclick = function(){
            var xhr = new XMLHttpRequest();
            xhr.open("post","03-uploadFile.php");
            //如果设置了请求头，那么文件数据无法正确的传递
            // xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
            var myform = document.querySelector("#form1");
            var formData = new FormData(myform);
            
            xhr.send(formData);
            xhr.onreadystatechange = function(){
                if(xhr.status == 200 && xhr.readyState == 4){
                    console.log(xhr.responseText);
                }
            }
        }
</script>
```

7.4 onprogress事件

```javascript
// 监听文件上传的进度:这个监听必须在send之前来设置
xhr.upload.onprogress = function(e){
	var current = e.loaded;//上传了多少
    var total = e.total;//总共
    var percent = current / total * 100 +"%";
    document.querySelector(".in").style.width = percent;
    document.querySelector("span").innerHTML = Math.floor(current / total * 100)+"%";
}
```

