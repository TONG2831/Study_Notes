#    HTML

## 1课程概要

- 课程介绍
- 从前端开发技术看，互联网的发展经历了三个阶段 
  - 第一阶段是web1.0，以内容为主的网络，主流的技术是HTML和CSS
  - 第二阶段是web2.0，以Ajax应用为主，主流的技术是JavaScript、DOM和异步数据请求
  - 第三阶段是web3.0，以HTML5+CSS3，使互联进入一个新时代

## 2开发工具

- 记事本
- eclipse、myEclipse
- editpulus、notepad++
- webstorm、sublime、Hbuilder、dreamweaver
- IDEAJ

## 3概述

### 3.1简介

- HTML是用来描述网页的一门语言
- HTML(Hyper Text Markup Language):超文本标记语言
- HTML 不是一种编程语言，而是一种标记语言 

### 3.2三个组织

- W3C (World Wide Web Consortium ，万维网联盟)，HTML官方,负责HTML各个版本的规范，目前负责发布HTML5的规范。
- WHATWG (Web Hypertext Application technology Working  Group,网页超文本应用技术工作组): 来自Apple、Google、FireFox等浏览器厂商人员组成，成立于2004年。 
- IETF(Internet Engineering Task Force:因特网工作任务组):负责开发Internet协议团队

### 3.3发展

![img](http://upload-images.jianshu.io/upload_images/5350300-e2726313397c6d7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- HTML最早是1995年从2.0开始的，实际上没有正式版的HTML1.0的官方规范。第一个HTML2.0的出现与规范不是出自w3c，而是由IETF制定的。到HTML2.0版本后版本才由W3C接手负责后续版本规范的制定
- 1996~1999时，HTML的版本由3.2到4.0，再由4.0变成4.0.1 ，经历了非常块的发展。问题是到了4.01后有了分歧
- 在2000年，W3C认识发生了改变 ，提出了XHTML概念，把文档变成XML方式。
- XHTMl存在一些问题
  - 把文档变成XML后，IE浏览器不能处理，当然8以上已经可以处理。
  - XML的语法很严格，如果整个页面有一个小错误，会导致正个页面都不显示。
- 2006年，W3C召开会议，进行反思，重回HTML方向
- 2007年，W3C投票通正式转向HTML方向，在WHTAWG上组建HTML5专家组

## 4HTML基础讲解

### 4.1声明 <!DOCTYPE>

HTML有多个不同版本，只有完成明白HTML的发展历史，才会体会每个时期的版本

- HTML5生成: html:5+tab /  !+tab

  ```html
  <!DOCTYPE html>
  <html lang="en">
  ```

- HTML4.01 

  - 工具初始化声明（无快捷方式）


- XHTML1.0 

  - Strict版本生成: html:xs+tab

  ```html
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
  <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
  ```

  - Transitional版本生成: html:xt+tab

  ```html
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
  <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
  ```

- XHTML1.1生成: html:xxs+tab

  ```html
  <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
  <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
  ```

### 4.2XHTML规范

#### 4.2.1XHTML的简介

1. 什么是XHTML

   是指可扩展的超文本标记语言。与HTML4.01几乎相同。是严格更纯净的HTML版本

2. 为什么作用XHTML

   为了代码的完整和良好性

3. 文档类型

   DTD 规定了使用通家标记语言的网页语法  

4. 三种XHTML的文档类型

   Strict严格的

   Transitional 过渡的

   FrameSEt 框架的

#### 4.2.2XHTML元素

1. XHTML的元素必须正确的嵌套。例如:`<div><span></div></span>`就是不合法的
2. XHTML元素必须始终关闭
3. XHTML标签名必须小写
4. XHTML文档必须有一个根元素,HTML文档所有信息必须在`<html>`标签中，文档元信息包含在`<head>`标签中，而网页显示的内容包含在`<body>`标签中

#### 4.2.3XHTML属性

1. XHTML属性必须使用小写
2. XHTML属性值必须用引号包围
3. XHTML属性是由属性名=“属性值”
4. 多个属性之间用空格隔开
5. 多个属性值之间用";"号隔开

#### 4.2.

- .html
- .htm

### 4.3HTML版的HelloWorld

基本结构

```html
<html>
	<head>
  	</head>
    <body>   
  	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
	</head>
    <body style='border:1px dashed red;font-size:36px;'>
		Hello World 
		你好世界
	</body>
</html>
```

## 5head下常用标签

- title	设置网页的标题

- meta 元标签、优化标签

  - charset=UTF-8设置当前页面内容编码格式 
    - utf-8  一个英文字符等于一个字节，一个中文（含繁体）等于三个字节。
    - unicode  一个英文等于两个字节，一个中文（含繁体）等于两个字节。
    - GBK  一个英文字母同样占用2个字节，一个中文文字占用2个字节。

  - name="" content=""
    - 关键字   keywords
    - 描述    description
    - 作者 author
    - 版本 copyright
  - http-evquiv=""   content=""  超文本传输协议标题信息
    - Conent-Type内容类型			text/html;charset=UTF-8
    - refresh 刷新                                 5秒

  ```html
  <meta charset="UTF-8">
  <meta name="keywords" content="" />
  <meta name="description" content="" />
  <meta http-equiv="content-type"  content="text/html; charset=UTF-8"/>
  <meta http-equiv="refresh" content="5;http://www.wanho.net">
  ```

## 6网页中常用标签

### 6.1标题标签

```
<!--标题标签 -->
<h1>标题1</h1>
<h2>标题2</h2>
<h3>标题3</h3>
<h4>标题4</h4>
<h5>标题5</h5>
<h6>标题6</h6>
```

### 6.2段落

```html
<p> 标记语言。</p>
<p>超文本标记语言</p>
```

### 6.3换行

```html
 标记语言。
 <br/> 超文本标记语言
```

### 6.4水平线

```
<hr/>
```

### 6.5字体样式

```html
<b>加粗</b>
<strong>强调2</strong>
<i>斜体</i>
<em>术语描述</em>
<del>删除线</del>
<u>下划线</u>
<font color="red" size="7" face="微软雅黑">字体</font>
```

### 6.6注释、实体和背景

- 注释

  ```
  <!--注释：ctrl+shift+/-->
  <!--联想：alt+/-->
  <!--自动补全：写一半，回车-->
  ```

- 实体

  ```html
  空格:					&nbsp;
  小于号					&lt;
  大于号					&gt;
  双引号					&quot;
  单引号					&apos;
  版权号：			   &copy;	
  注册号：			   &reg;	
  &符号                  &amp;   
  ```

- ### 颜色 color 

  - 颜色名
    - gray
    - pink
    - linghtgray
  - 十六进制(由六位组)
    - `#FFFF00`
    - `#FFFFFF`
    - `#000000`

  ​

### 6.7图像标签

```
<!--图片-->
<!--相对路径、绝对路径（外部网）-->
<!--..上一目录;.当前目录-->
<img src="img/dlrb.jpg" height="300px" width="1000px" />
<img src="../img/dlrb.jpg" height="100px" width="100px" alt="胖迪"/>`
```

- src:图片源
- alt:当图片源不存在的时候，显示的文字 
- width:设置图片宽度
- height:设置图片高度

## 7链接标签 

### 7.1不同页面的链接

- 当前窗体(默认)

  ```html
  <a href="http://www.wanho.net" target="_self">万和</a>
  ```

- 新窗体 打开 

  ```html
  <a href="http://www.wanho.net" target="_blank">万和</a>
  ```

### 7.2同一个页面的不同部分（锚点）

1. 在页面设置锚

   ```html
   <a href="#ta">aaa</a>
   ```

2. 在页面设置点

   ```html
   <p><a name="ta"></a>a1</p>
   <p>a2</p>
   <p>a3</p>
   ```

### 7.3链接到邮箱

```html
<a href="mailto:邮箱名" target="_blank">发送邮件</a>
```

##8列表标签（都可以嵌套） 

常用标签

`ol` 有序列表 

`ul`无序列表

`li`列表项

`dl` 定义列表

`dt`定义标签

`dd`定义描述

### 8.1有序列表

- 标签 `<ol><li></li><li></li></ol>`
- 属性:   
  - 名type 值: A,a,I,i,1
  - 名start 值 2

###8.2无序列表

- 标签  `<ul><li></li><li></li></ul>`
- 属性 disc,circle,square

###8.3定义列表 

- 标签 `<dl><dt></dt><dd></dd></dl>`

```html
 <!-- ol>li{$}*5 -->
    <ol type="1" start="1">
        <li>abc</li>
        <li>abc</li>
        <li>abc</li>
        <li>abc</li>
        <li>abc</li>
    </ol>
    <hr/>
    <ul type="disc">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
    <ul type="circle">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
    <ul type="square">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
    <hr/>
    <dl>
        <dt>标题1</dt>
        <dd>描述1</dd>
        <dt>标题2</dt>
        <dd>描述2</dd>
    </dl>
```

## 9表格

常用标签

- 表格			`<table></table>`
- 表格标题                `<caption></caption>`
- 表格行                    `<tr></tr>`
- 列标题                    `<th></th>`
- 列（数据）            `<td></td>`
- 表格的页眉            `<thead></thead>`
- 表格的主体           `<tbody></tbody>`
- 表格的页脚           `<tfoot></tfoot>`
- 表格的列属性        colspan  rowspan

常用属性

- 边框		border="1px"
- 宽度                width="80%"
- 水平对齐        align="center"
- 单元格之间距离  cellspacing="0px"   
- 内容距边框的距离 cellpadding="0px"

```html
<table border="1" width="80%" align="center" cellspacing="0" cellpadding="0">
        <caption>
            <h1>学生信息管理</h1>
        </caption>
        <thead>
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>地址</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1001</td>
                <td>张三</td>
                <td>18</td>
                <td>男</td>
                <td>南京</td>
            </tr>
            <tr>
                <td>1001</td>
                <td>张三</td>
                <td>18</td>
                <td>男</td>
                <td>南京</td>
            </tr>
        </tbody>
    </table>
    <hr>
    <table border="1px" width="80%" align="center" cellspacing="0" cellpadding="0">
        <tr>
            <td rowspan="3">一季度</td>
            <td>一月</td>
            <td colspan="2" align="center">支出</td>
        </tr>
        <tr>
            <td>二月</td>
            <td>必需</td>
            <td>奢侈</td>
        </tr>
        <tr>
            <td>三月</td>
            <td></td>
            <td></td>
        </tr>
    </table>
```

## 10内联样式与块样式（块级元素和行内元素） 

- 内联样式: 内联元素通常用不会以新行开始 
  - 如`<b><a><img><span>`
- 块样式: 块元素会独占一行
  - 如:`<h1><p><ul><div>`

## 11框架 

### 11.1框架集 frameset

常用标签

- `<frameset>`
- `<frame>`

 常用属性

​	rows		按行设置框架集

​	cols			按列设置框架集

​	border		设置框架集边框

​	noresize        设置框架不能被拖拽

​	name	     给框架取名，为在其它链接在此框架中显示 `<a href="xx" target="框架名">xxx</a>`

```html
<frameset rows="20%,*" border="1">
    <frame src="top.html" noresize />
    <frameset cols="30%,*">
        <frame src="left.html" />
        <frame src="info.html" name="mainFrame" />
    </frameset>
</frameset>
<noframes>
    <body>
        当前浏览器不支持框架
    </body>
</noframes>
```

### 11.2内联框架 iframe

常用标签：`<iframe>`

常用属性: 

​	frameborder  窗体的边框

​	width	宽度

​	name	名称，为在其它链接在此框架中显示 `<a href="xx" target="框架名">xxx</a>`

```
 <iframe src="top.html" frameborder="0" width="100%"></iframe>
 <br/>
 <iframe src="left.html" frameborder="0" width="30%" scrolling="no"></iframe>
 <iframe src="info.html" frameborder="0" name="mainFrame"></iframe>
```

## 12表单标签 `<form></form>` 

```
<body>
	<form action="success.html" method="get">
		<label>用户名</label><input type="text" name="username" /><br />
		<label>密码</label> <input type="password" name="pwd" /><br />
		<label>重复密码</label><input type="password" name="repwd" /><br />
		<label>电话号码</label><input type="text" name="phone" /><br />
		<input type="button" value="立即注册01" /><br />
		<input type="submit" value="立即注册02" /><br />
		<button> 立即注册03</button>
	</form>
</body>
```

- 作用: 提交表单数据

- 常用表单标签

  - 表单	`<form>`

  - 输入框   `<input>`

    - 文本	text
    - 密码框    password
    - 文件        file
    - 单选按钮 radio
    - 多选按钮 checkbox
    - 图片  image
    - 隐藏  hidden
    - 普通按钮  button
    - 提交按钮 submit
      - 重置按钮 

  - 文本域  `<textarea>`

  - 标签    `<label>`

  - 按钮    `<button>`

  - 下拉框  `<select>`

    - 选项框  `<option>`

  - 列表框 `<select multiple>`

  - 定义域`<fieldset>`

    - 域的标题`<legend>`



```
<fieldset>
	<legend>用胡注册表单</legend>
	<form action="success.html" method="get">
		<table border="1" cellpadding="0" cellspacing="0" width="80%" align="center">
			<tr>
				<td><label>序号</label></td>
				<td>
					<input type="hidden" name="id" value="9527" />
					<input type="text" name="number" value="1" readonly/>
				</td>
			</tr>
			<tr>
				<td><label>用户名</label></td>
				<td>
					<input type="hidden" name="id" value="9527" />
					<input type="text" name="username" />
				</td>
			</tr>
			<tr>
				<td><label>密码</label></td>
				<td><input type="password" name="pwd" /></td>
			</tr>
			<tr>
				<td><label>性别</label></td>
				<td>
					<input type="radio" name="sex" value="man" checked/> 男 
					<input type="radio" name="sex" value="woman" /> 女
				</td>
			</tr>
			<tr>
				<td><label>爱好</label></td>
				<td><input type="checkbox" name="hobby" value="mz" />妹子
					<input type="checkbox" name="hobby" value="xx" />学习
					<input type="checkbox" name="hobby" value="dyx" />打游戏
					<input type="checkbox" name="hobby" value="qdm" />敲代码
				</td>
			</tr>
			<tr>
				<td><label>故乡</label></td>
				<td>
					<select name="home" multiple>
						<option value="ah">安徽</option>
						<option value="fj">福建</option>
						<option value="gd">广东</option>
						<option value="js" selected>江苏</option>
					</select>
				</td>
			</tr>
			<tr>
				<td><label>头像</label></td>
				<td><input type="file" name="picture" /></td>
			</tr>
			<tr>
				<td><label>个人简介</label></td>
				<td><textarea cols="50" rows="5"></textarea></td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="submit" value="立即注册02" />
					<input type="image" src="img/zbz.jpg" />
					<img src="img/zbz.jpg" />
					<input type="reset" value="重置" />
				</td>
			</tr>
		</table>
	</form>
</fieldset>
```
























