# html5

## 1.新特性

- 绘画：canvas标签
- 媒介：video和audio元素
- 存储：本地离线存储
- 新元素：article,footer,header,nav,section
- 新控件：calendar,date,time,email,url,search
- 浏览器：Safari,Chrome,Firefox,Opera,IE9

## 2.改进的地方

- DOCTYPE 声明

```
<!DOCTYPE html>
```

- 编码格式

```
<meta charset="utf‐8">
```

- 检测是否支持HTML5

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF‐8">
		<title>Document</title>
		<style>
			#myCanvas {
				background: red;
				width: 200px;
				height: 100px;
			}
		</style>
	</head>
	<body>
		<canvas id="myCanvas">该浏览器不支持HTML5画布</canvas>
	</body>
</html>
```

## 3.使用HTML5编写简单的Web页面

```
<!DOCTYPE html>
<meta charset="utf‐8">
<title>第一个HTML5页面</title>
<p>Hello,World</p>
```

虽然在编写代码时省略了一些标记，但在浏览器进行解析时，将会自动进行添加
考虑到代码的可维护性，在编写代码时，应该尽量增加这些在HTML5中可选的元素，从而最大限定的实现页面代码的完整

## 4.使用HTML5结构化元素

```
<head>
	<style>
		header,
		nav,
		article,
		footer {
			margin: 10px 0;
		}
		header {
			border: 1px solid #ccc;
			width: 100%;
			height: 60px;
		}	
		nav {
			border: 1px solid #ccc;
			width: 20%;
			height: 500px;
			float: left;
		}
		article {
			border: 1px solid #ccc;
			height: 500px;
			width: 78%;
			float: right;
		}	
		footer {
			clear: both;
			border: 1px solid #ccc;
			height: 60px;
		}
	</style>
</head>
<body>
	<header>万和欢迎你。。。</header>
	<nav>导航</nav>
	<article>内容</article>
	<footer>脚部</footer>
	<!--<div id="d1">万和欢迎你。。。</div>-->
	<!--<div id="d2">导航</div>-->
	<!--<div id="d3">内容</div>-->
	<!--<div id="d4">底部说明</div>-->
</body>
```

## 5.结构元素

### 5.1.header元素

一种具有引导和导航作用的结构元素，例如搜索表单或相关的logo图片。顾名思义，整个页面的标题应该放在页面的开头。

```
<header><h1>页面标题</h1></header>
```

一个网页内并未限制header元素的个数，可拥有多个，可以为每个内容区块加一个header元素
在HTML5中，一个header元素通常包括至少一个heading元素（h1~h6），也可以包括hgroup、table、from、nav

### 5.2.hgroup元素

标题及其子标题进行分组的元素。

```
<header>
  <hgroup>
    <h1>HTML5 食谱</h1>
    <h2>美味 HTML5 配方</h2>
  </hgroup>
</header>
```

### 5.3. footer元素

表示整个页面或页面中一个内容区块的脚注。一般来说，它会包含创作者的姓名、文档的创作日期以及创建者联系信息。
与header元素一样，一个页面中也没有对footer元素的个数

### 5.4. address 元素:

address 元素用来在文档中呈现联系信息，包括文档作者或文档维护者的名字、他们的网站链接、电子邮箱、真实地址、电话号码等。

```
<header><h1>html5 address 示例 www.169it.com</h1></header>
<p>这里是主体...</p>
<footer>
  	作者：www.169it.com
    <address>
        <ul>
            <li>网址：http://www.169it.com</li>
            <li>QQ：10000</li>
            <li>邮件：web@169it.com</li>
        </ul>
    </address>
</footer>
```

## 6.多媒体video和audio元素

- src属性用于指定媒体数据的URL地址。
- autoplay属性用于指定媒体是否在页面加载后自动播放
- loop属性：用于指定是否循环播放视频或音频
- controls属性 指定是否为视频或音频添加浏览器自带的播放用的控制条。
- width属性与height属性（ video元素独有属性）用于指定视频的宽度与高度（以像素为单位）

```
<body>
	<!--写法一-->
	<video width="300px" height="300px" controls>
		<source src="vedio/movie.mp4"></source>
	</video>
	<!--写法二-->
	<video src="vedio/movie.mp4" controls loop></video>
	<audio src="vedio/Truth of the Legend.mp3" controls></audio>
</body>
```

## 7.表单

### 7.1input类型

- email类型用于应该包含 e-mail 地址的输入域
- url 类型用于应该包含 URL 地址的输入域。
- number 类型用于应该包含数值的输入域。
- range 类型用于应该包含一定范围内数字值的输入域。
- date
- time
- dateTime
- dateTime-Local
- month
- week

### 7.2相关属性

- autocomplete 属性规定 form 或 input 域应该拥有自动完成功能
- autofocus 属性规定在页面加载时，域自动地获得焦点。
- list 属性规定输入域的 datalist
- pattern 属性规定用于验证 input 域的模式（pattern）
- placeholder 属性提供一种提示（hint），描述输入域所期待的值。
- required 属性规定必须在提交之前填写输入域（不能为空）
- checkValidity显式验证法
- 使用setCustomValidity方法自定义错误信息

```
<!--html5：新属性、input新type类型-->

<form action="success.html" method="get">
	<p>
		<!--placeholder:提示，描述输入域所期待的值-->
		<!--autofocus:自动获得焦点-->
		<label>用户名：</label><input type="text" name="username" placeholder="请输入用户名" autofocus/>
	</p>
	<p>
		<label>密码：</label><input type="password" name="pwd" placeholder="请输入密码" disabled="disabled" />
	</p>
	<p>
		<!--required 必填-->
		<!--pattern 用于验证 input 域的模式-->
		<label>手机：</label><input type="text" name="phone" required pattern="[A-z]{3}" />
	</p>
	<p>
		<!--autocomplete 规定输入字段是否应该启用自动完成功能。on、off-->
		<label>E-mail:</label><input type="email" name="email" autocomplete="off" />
	</p>
	<p>
		<label>URL:</label><input type="url" name="url" />
	</p>
	<p>
		<label>数字:</label><input type="number" name="sz" />
	</p>
	<p>
		<label>出生时间:</label><input type="date" name="date" />
		</label><input type="datetime" name="datetime" />
		</label><input type="datetime-local" name="date" />
		</label><input type="month" name="date" />
		</label><input type="time" name="date" />
		</label><input type="search" name="date" />
		</label><input type="image" src="img/ym.jpg" />
		<input type="submit" value="注册" />
	</p>
</form>
```









