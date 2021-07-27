# jQuery

## 1. 简介

于2006年由Jhon Resig美国人创建

它是一种流行JavaScript类库

它的思想是write less,do more(写的更少，做的更多)

## 2. 下载

网址: 进入jQuery官网：http://www.jquery.com

下载三个文件

- jquery-3.2.1.js

- jquery-3.2.1.min.js

- jquery-3.2.1.min.map

  ```
  map文件就是压缩和混淆过程产生的产物，它保存了压缩前的标示符和压缩后的 标示符的映射
  ```

## 3. 初识jQuery

```html
 <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
 <script type="text/javascript">
    window.onload = function() {
        var trs = document.getElementsByTagName("tr");
        for (var i = 0; i < trs.length; i++) {
            if ((i + 1) % 2 == 0) {
                trs[i].style = "background-color:#Ecc";
            } else {
                trs[i].style.backgroundColor = "#cfc";
            }
        }
    }
    $(function() {
        $("tr:odd").css('background-color', '#8fc');
        $("tr:even").css('background-color', '#2f2');
    });
</script>
<body>
    <table border="1" cellpadding="5" cellspacing="0" width="50%" align="center">
        <tr>
            <td>1</td>
            <td>1</td>
        </tr>
        <tr>
            <td>2</td>
            <td>2</td>
        </tr>
        <tr>
            <td>3</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>4</td>
        </tr>
        <tr>
            <td>5</td>
            <td>5</td>
        </tr>
    </table>
 </body>
```

## 4.  jQuery函数的入口

在DOM加载完成后执行

代码形式如下

```javascript
$(document).ready(function(){
  
});
```

简写如下

```javascript
$(function(){
  
});
```

上面代码与JavaScript的下代码类似，但本质又不太一样

```javascript
window.onload=function(){
  
}
```

JavaScript的window.onload与 jQuery人$(document).ready()区别？

- 前者在页面所有的元素(大图片、视频、音频)都加载完才执行；而后者是在dom加载完后就执行

- 前者在一个页面中只会有一个函数生效，最后的覆盖前面的；而后者可以有多个同时生效

- 前者没有简写的，后者可以简写

  		<script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
  		<script type="text/javascript">
  			window.onload = function() {
  				alert(1);
  			}
  	
  			//此函数覆盖了上面的，只有此函数生效
  			window.onload = function() {
  				alert(2);
  			}
  	
  			$(function() {
  				alert(3);
  			});
  			$(function() {
  				alert(4);
  			});
  		</script>

## 5. DOM对象与jQuery对象

这两者是不相等的对象，使用时注意区分，不能混用

- DOM对象转成jQuery对象 

  - 使用$()

  ```javascript
  var nameNode = document.getElementById("uname");
  var $nameNode = $(nameNode);
  ```

- jQuery对象转成DOM对象

  - 使用 []  
  - 使用 .get()

  ```javascript
  var $uname = $("#uname");
  var uname = $uname[0];
  var uname = $uname.get(0);
  ```

```
<input type="text" id="nameId" name="name" value="张三" />
<input type="text" id="nameId" name="name" value="张三" />

<script type="text/javascript">
	var inputNode = document.getElementById("nameId");
	console.log(inputNode);
	$(function() {
		/*jquery自己获取对象*/
		var input01 = $("input[name='name']");
		input01.val("ww");

		/*dom对象转jquery对象*/
		var input02 = $(inputNode);
		console.log(input01);
		console.log(input02);

		/*jquery对象转DOM对象*/
		var inputDom01 = input01[0];
		var inputDom02 = input01.get(0);
		console.log(inputDom01);
		console.log(inputDom02);
	});
</script>
```

## 6. jQuery选择器

### 6.1. 基本选择器

基本选择器是 jQuery 中最常用的选择器, 也是最简单的选择器, 它通过元素 id, class 和标签名来查找 DOM 元素(在网页中 id 只能使用一次, class 允许重复使用).

- 标签选择器
- ID选择器$("#xx")
- class选择器$(".xx")
- 并集选择器$("#xx,.yy")
- 交集选择器$("div.class")
- 全局匹配选择器$("*")

```
<body>

	<p>你喜欢的城市</p>
	<ul id="city">
		<li id="bj" name="beijing" class="active">北京</li>
		<li class="classLi">上海</li>
		<li class="classLi">广州</li>
		<li class="classLi">深圳</li>
	</ul>
	<hr/>
	<p>你的喜欢的游戏</p>
	<ul id="game">
		<li id="rl" class="active">红警</li>
		<li class="classLi">连连看</li>
		<li class="classLi">扫雷</li>
		<li class="classLi">纸牌</li>
	</ul>
	<br/>
	<hr/>
	<label>gender:</label>
	<input type="radio" name="gender" value="male">男
	<input type="radio" name="gender" value="female">女

	<script type="text/javascript">
		$(document).ready(function() {
			//$("#bj").text("南京");
			var $liBj = $("#bj");
			$liBj.text("南京");
			var $liByClass = $(".classLi");
			console.log($liByClass.length);
			var $liByLi = $("li");
			console.log($liByLi.length);
			/*并集*/
			var $bingjie = $("#bj,#rl");
			console.log($bingjie.length);
			/*交集*/
			var $jiaojie = $("li#rl");
			console.log($jiaojie.length);
		});
	</script>
</body>
```

### 6.2.层次选择器

如果想通过 DOM 元素之间的层次关系来获取特定元素, 例如后代元素, 子元素, 相邻元素, 兄弟元素等, 则需要使用层次选择器.

- 后代选择器  （空格）
- 直接子选择器       ( > )
- 同辈后相邻的选择器(+)   next( )
- 同辈后相邻的选择器(~)   nextAll( )
  - ("#prev~div")选择器能选中#prev后所有的同辈元素，而jQuery中的方法 siblings(),选择前与同辈

```html
 <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
    <script type="text/javascript">
    $(function() {
        //后代选择器
        //$("#article li").css('color', 'pink');
        //直接子代选择器
        //$("#article>li").css('color', 'pink');
        //下一兄弟选择器
        //$("#item+li").css('color', 'pink');
        //$("#item").next().css('color', 'pink');
        //下所有兄弟选择器
        //$("#item~li").css('color', 'pink');
        //$("#item").nextAll("li").css('color', 'pink');
        $("#item").siblings("li").css('color', 'pink');
    });
    </script>
</head>

<body>
    <!-- ul>li{$}*5 -->
    <ul id="article">
        <li>1</li>
        <li id="item">2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <ul>
            <li>11</li>
            <li>12</li>
            <li>13</li>
        </ul>
    </ul>
</body>
```

### 6.3. 属性选择器

属性选择器的过滤规则是通过元素的属性来获取相应的元素

```html
  <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
    <script type="text/javascript">
    $(function() {
        //只包含属性名;
        //$("li[class]").css('background-color', '#C1c');
        //属性名=值;
        //$("li[class=i1]").css('background-color', '#C1c');
        //属性名!=值;
        //$("li[class!=i1]").css('background-color', '#C1c');
        //属性名以值开头;
        //$("li[title^=t]").css('background-color', '#C1c');
        //属性名以值结束;
        //$("[class$=1]").css('background-color', '#C1c');
        //属性名包含值;
        //$("[class*=i]").css('background-color', '#C1c');
        //属性名1=值1且属性名2=值2;
        $("li[class*=i][title$=1]").css('background-color', '#C1c');
    });
    </script>
</head>
<body>
    <ul>
        <li class="i1">111</li>
        <li class="i2">112</li>
        <li class="i3">113</li>
        <li class="i4">114</li>
        <li class="i5">115</li>
        <li class="i6" title="t1">116</li>
        <li class="i7" title="t2">117</li>
        <li class="i8" title="t3">118</li>
    </ul>
    <span class="s1">ssssssssssssssssssssssssssssssssssss</span>
</body>
```

### 6.4. 过滤选择器

#### 6.4.1. 基本过滤选择器

```html
 <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
    <script type="text/javascript">
    $(function() {
        // 第一个li
        //$("ul>li:first").css('background-color', '#f40');
        // 最后一个li
        //$("ul>li:last").css('background-color', '#f40');
     	// 等于第四个li
        //$("ul>li:eq(3)").css('background-color', '#f40');
        // 不是
        //$("ul>li:not(li:eq(2))").css('background-color', '#f40');
        // 奇数
        //$("ul>li:even").css('background-color', '#f40');
        // 偶数
        //$("ul>li:odd").css('background-color', '#f40');
        // 大于第三个li
        //$("ul>li:gt(2)").css('background-color', '#f40'); 
        // 小于第四个li
        //$("ul>li:lt(3)").css('background-color', '#f40');
        // 标题
        //$(":header").css('background-color', '#f40');
        //焦点:
        $("input[type='text']").click(function(event) {
            $(":focus").css('background-color', '#f40');
        });
    });
    </script>
</head>

<body>
    <!-- div>h2+h1+ul>li{1$}*8 -->
    <div>
        <h2>排行</h2>
        <h1>音乐</h1>
        <ul>
            <li>11</li>
            <li>12</li>
            <li>13</li>
            <li>14</li>
            <li>15</li>
            <li>16</li>
            <li>17</li>
            <li>18</li>
        </ul>
    </div>
    <input type="text">
    <input type="button" value="启动">
    <div id="dong">
        love you hate you
    </div>
```

#### 6.4.2. 可见过滤选择器

- 可见性过滤选择器是根据元素的可见和不可见状态来选择相应的元素


- 可见选择器 :hidden 不仅包含样式属性 display 为 none 的元素, 也包含文本隐藏域 (<input  type=“hidden”>)和 visible:hidden 之类的元素


- :visible  可见   hide()
- :hidden 隐藏   show()
- 切换显示的方法toggle()

```html
<body>
	<p id="p_show">点我显示</p>
	<p id="p_hide">点我隐藏</p>
	<p id="p_hide">点我切换</p>
	<div style="border:1px solid red;">
		这里是显示的文本block
	</div>
	<script type="text/javascript" src="js/jquery-3.2.1.js"></script>
	<script type="text/javascript">
		$(function() {
			/*click点击事件*/
			$("#p_show").click(function() {
				//alert("123")
				//$("div").css("display","block");
				/*:hidden 隐藏   show()*/
				$("div:hidden").show(3000);
				console.log("123");
			});

			$("p:eq(1)").click(function() {
				//$("div").css("display","none");
				/*:visible  可见   hide()*/
				$("div:visible").hide(3000);
			});

			$("p:eq(2)").click(function() {
				/*toggle()：切换显示*/
				$("div").toggle(3000);
			});
		});
	</script>
</body>
```

#### 6.4.3. 内容过滤选择器

内容过滤选择器的过滤规则主要体现在它所包含的子元素和文本内容上

```html
<body>

	<div>
		<span>dididididid</span>
		<!--<div>subdiv</div>-->
		<div>1111</div>
	</div>
	<div style="height:40px;"></div>
	<div>
		<span id="spanId">mini</span>
		<span class="mini">mini</span>
	</div>
	<div>abcdefg</div>
	<script type="text/javascript">
		$(function() {
			/*包含xx内容*/
			$("div:contains('di')").css("background-color", "#ccc");
			/*不包含内容*/
			//$("div:empty").css("background-color", "yellow");
			//$("div:not(:empty)").css("background-color","yellow");
			/*内容里面包含选择器 */
			$("div:has('.mini')").css("color", "red");
			$("div:has('#spanId')").css("color", "red");
			$("div:parent").css("background-color", "orchid");
		});
	</script>
</body>
```

#### 6.4.4.  子元素过滤选择器

子元素过滤选择器

```html
<script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
<script type="text/javascript">
	$(function() {
		//$("li:eq(2)").css("color","red");
		/*li ,做为第二个儿子*/
		$("li:nth-child(2)").css("color", "red");
		/*li ,做为第一个儿子*/
		$("li:first-child").css("background-color", "green");
		/*li ,做为最后一个儿子*/
		$("li:last-child").css("background-color", "#ccc");
		/*第一个li*/
		//$("li:first").css("font-size", "80px");
		/*li 作为独生子*/
		$("li:only-child").css("background-color", "blue");
	});
</script>
</head>
<body>
    <div>
        <ul>
            <span>aaa</span>
            <li class="one">1</li>
            <li class="one">2</li>
            <li class="one">3</li>
            <li class="one">4</li>
            <li class="one">5</li>
        </ul>
        <ul>
            <li>other</li>
        </ul>
    </div>
</body>
```

#### 6.4.5.  表单选择器

:input 
:text 
:password 
:radio 
:checkbox 
:submit 
:image 
:reset 
:button 
:file 
:hidden 

#### 6.4.6. 表单对象属性过滤选择器

```html
<body>
	<form action="" method="post">
		<table border="1" width="60%" align="center">
			<tr>
				<td>
					<label for="uname">用户名</label>
				</td>
				<td>
					<input type="text" id="uname" name="uname" disabled>
				</td>
			</tr>
			<tr>
				<td>
					<label for="name">姓名</label>
				</td>
				<td>
					<input type="text" id="name" name="name">
				</td>
			</tr>
			<tr>
				<td>
					<label for="upass">密码</label>
				</td>
				<td>
					<input type="password" disabled id="upass" name="upass">
				</td>
			</tr>
			<tr>
				<td>爱好</td>
				<td>
					<input type="checkbox" value="travel">旅游
					<input type="checkbox" value="reading" checked>读书
					<input type="checkbox" value="music">音乐
				</td>
			</tr>
			<tr>
				<td>出生年月</td>
				<td>
					<select name="birthDate" id="birthDate" multiple>
						<option value="1994">1994</option>
						<option value="1995" selected>1995</option>
						<option value="1996">1996</option>
					</select>
				</td>
			</tr>
			<tr>
				<td colspan="2">
					<input type="button" value="设置用户名和姓名框的值">
					<input type="button" value="设置密码框的值">
					<!--<input type="button" value="设置复选框内容" onclick="show()">-->
					<input type="button" value="控制台输出复选框内容">
					<input type="button" value="控制台输出下拉框内容">
				</td>
			</tr>
		</table>
	</form>
	<script type="text/javascript">
		//			function show() {
		//				var len = $("input[type='checkbox']:checked").length;
		//				var $checkbox = $("input[type='checkbox']:checked");
		//				/*each 遍历   index下标  ele 值*/
		//				$.each($checkbox, function(index, ele) {
		//					//console.log($checkbox[index]);
		//					console.log(ele.value);
		//				});
		//			}
		$(function() {
			$("input[type='button']:first").click(function() {
				$("input[type='text']:disabled").val("zhangsan");
				$("input[type='text']:enabled").val("张三");

			});
			$("input[type='button']:eq(1)").click(function() {
				$("input[type='password']:disabled").val("zhangsan");
				//$("#upass").val("zhangsan");
			});
			$("input[type='button']:eq(2)").click(function() {
				var len = $("input[type='checkbox']:checked").length;
				var $checkbox = $("input[type='checkbox']:checked");
				/*each 遍历   index下标  ele 值*/
				$.each($checkbox, function(index, ele) {
					//console.log($checkbox[index]);
					console.log(ele.value);
				});
			});
			$("input[type='button']:eq(3)").click(function() {
				var len = $("option:selected").length;
				var arr = [];
				/*each 遍历   index下标  ele 值*/
				$.each($("option:selected"), function(index, ele) {
					console.log(ele.value);
					arr.push(ele.value);
				});
				console.log(arr.join("-"));
				//1994-1995-1996
			});
		})
	</script>
</body>
```

## 7. 事件

### 7.1. 基础事件

1. window事件
2. 鼠标事件
   1. mouseover()
   2. mouseout()
   3. mousedown()
   4. mouseup()
   5. mouseenter()
   6. mouseleave()
   7. mousemove()

```
<body>

	<p style="border: 1px solid red;">我是一个P...</p>
	<p style="border: 1px solid red;">我是一个P...</p>
	<p style="border: 1px solid red;">我是一个P...</p>
	<p style="border: 1px solid red;">我是一个P...</p>
	<p style="border: 1px solid red;">我是一个P...</p>
	<div style="border: 1px solid red;height: 150px;">
		<div style="border: 1px solid blue;height: 50px; width: 100px;">
		</div>
	</div>
	<button>点</button>
	<button>点击</button>
	<hr/>
	<input type="text" /><span></span>
	<script type="text/javascript">
		$(function() {
			$("button:eq(0)").mousedown(function() {
				$("p").toggle();
			});
			$("button:eq(1)").click(function() {
				$("p").toggle();
			});
			$("p").mouseenter(function() {
				$(this).css("background-color", "#ccc");
			});
			$("p").mouseleave(function() {
				$(this).css("background-color", "#fff");
			});
			/*$("div").mousemove(function(e) {
				$(this).text(e.pageX + ", " + e.pageY);
			});*/
			var i = 0;
			$("input").keypress(function() {
				$("span").text(i += 1);
			});
		})
	</script>
</body>
```

1. 键盘事件
   1. keydown()
   2. keyup()
   3. keypress()
2. 表单事件
   1. focus()
   2. blur()
   3. submit()
   4. change()
   5. select()
   6. click()
   7. dbclick()

- 绑定事件
  - bind("事件名",function(){});
  - on("事件名",function(){});
  - one("事件名",function(){});
- 移除事件
  - unbind()没有事件名，则移除所有事件
  - unbind("事件名"); 移除当前事件名的事件
  - off("事件名")
- 模拟调用事件
  - trigger('事件名');

```html
<body>
	<form action="">
		<select name="" id="">
			<option value="1">first</option>
			<option value="2">second</option>
			<option value="3" selected>third</option>
		</select>
	</form>
	<button> click me1</button>
	<br/>
	<button> click me2</button>
	<br/>
	<button> click me3</button>
	<br/>
	<button> click me4</button>
	<br/>
	<button> dbclick</button>
	<br/>
	<button> cancel click </button>
	<br/>
	<button> cancel click </button>
	<br/> 用户名：
	<input type="text" /><span></span>
	<script type="text/javascript">
		//值改变事件
		$("select").change(function() {
			console.log($(this).children("option:selected").text())
		});
		//绑定点击事件1
		$("button:eq(0)").click(function() {
			alert("上课别睡觉1~~")
		});
		 //绑定点击事件2
        $("button:eq(1)").bind('click', function() {
            alert("上课别睡觉2~~");
        });
        //绑定点击事件3
        $("button:eq(2)").on('click', function() {
            alert("上课别睡觉3~~");
        });
         //绑定事件4，只能触发一次
        $("button:eq(3)").one('click', function(event, e1, e2) {
            alert("上课别睡觉4~~"+ event.type + e1 + e2 );
        });
        //绑定双击事件
      	$("button:eq(4)").dblclick(function() {
			alert("上课别再睡觉啦~~")
		});
		//模拟调用事件
        //$("button:eq(3)").trigger('click', ['a', 'b']);
        //移除事件5;unbind("事件名"):若没有事件名，则移除所有事件
        $("button:eq(5)").click(function() {
            $("button").unbind('click');
        });
        //移除事件6
        $("button:eq(6)").click(function() {
            $("button").off('click');
        });
        //离开焦点事件
		$("input:eq(0)").blur(function() {
			$(this).css("background-color","white")
		});
		//获得焦点事件
		$("input:eq(0)").focus(function() {
			$(this).css("background-color","antiquewhite")
		});
	</script>
</body>
```

### 7.2. 复合事件

hover():模拟光标悬停事件。当光标移动到元素上时，会触发第一个函数，当光标移出这个元素时，会出发第二个函数

toggle()：用于模拟鼠标的单击事件，第一次单击元素时，触发的第一个函数，当再次单击会触发第二个函数，依次类推，直到执行最后一个函数（过期）

- hover()  相当于mouseover()和mouseout()
- toggle() 相当于鼠标的连续点击事件（在1.9后过期），和元素的隐藏和显示

```html
<body>
	<ul>
		<li>1</li>
		<li>2</li>
		<li>3</li>
		<li>4</li>
		<li>5</li>
		<li>6</li>
		<li>7</li>
		<li>8</li>
	</ul>
	<script type="text/javascript">
		$(function() {
			//hover()  相当于mouseover()和mouseout()
			$("li:eq(4)").hover(function() {
				$(this).css("background-color", "green");
			}, function() {
				$(this).css("background-color", "#fff");
			})
			//等价于上面的效果
			$("li:eq(4)").mouseover(function() {
				$(this).css('background-color', '#F40');
			}).mouseout(function(event) {
				$(this).css('background-color', "#fff");
			});
		});
	</script>
</body>
```

### 7.3. 事件冒泡

什么是事件冒泡？

（3个内嵌div都是事件，促发最里面div的事件，会导致外面2个div的事件也会触发。）

- 事件会按照DOM层次结构像水泡一样不断向上直到顶端
- 解决
  - return false
  - event.stopPropagation();
  - event.cancelBubble=true;

```
<head>
	<style type="text/css">
		#d1 {
			border: 1px solid red;
			width: 600px;
			height: 600px;
			margin: 0 auto;
		}	
		#d2 {
			border: 1px solid green;
			width: 300px;
			height: 300px;
			margin: 0 auto;
		}	
		#d3 {
			border: 1px solid blue;
			width: 100px;
			height: 100px;
			margin: 0 auto;
		}
	</style>
</head>
<body>
	<div id="d1">
		<div id="d2">
			<div id="d3"></div>
		</div>
	</div>
	<script type="text/javascript" src="js/jquery-3.2.1.js"></script>
	<script type="text/javascript">
		$(function() {
			$("#d1").click(function() {
				alert("1");
			});
			$("#d2").click(function() {
				alert("2");
				event.stopPropagation();
			});
			$("#d3").click(function() {
				alert("3");
				//return false;
				event.cancelBubble=true;
			});
		});
	</script>
</body>
```

- 巧秒的使用事件冒泡（不演示了）

  ```html
  <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
      <script type="text/javascript">
      $(function() {
          
          //当前li一个色，其它li另一色
          //使用冒泡写法
          $("ul").mouseover(function(event) {
              $(event.target).css('background-color', '#f40').siblings().css('background-color', '#999');
          });

          //原始写法
          $("li").mouseover(function(event) {
              $(this).css('background-color', '#f40').siblings().css('background-color', '#999');
              //event.stopPropagation();
          });
      });
      </script>
  </head>

  <body>
      <ul>
          <li>1</li>
          <li>2</li>
          <li>3</li>
          <li>4</li>
          <li>5</li>
      </ul>
  </body>
  ```

```javascript
 //当前li一个色，其它li另一色
//使用冒泡写法
$("ul").mouseover(function(event) {
  $(event.target).css('background-color', '#f40').siblings().css('background-color', '#999');
});x//当前li一个色，其它li另一色 
```

```javascript
 //原始写法，不使用冒泡
$("li").mouseover(function(event) {
  $(this).css('background-color', '#f40').siblings().css('background-color', '#999');
  //event.stopPropagation();
});
```

## 8. 动画

### 8.1. 显示和隐藏

- show()
- hide()
- toggle()

### 8.2. 淡入和淡出

- fadeIn() 淡入
- fadeOut() 淡出
- fadeToggle() 淡出入
- fadeTo() 淡入到透明度

### 8.3. 滑动动画

- slideDown()
- slideUp()
- slideToggle()

### 8.4. 自定义动画

animate({参数},毫秒)

```html
<body>
	<img src="img/zbz.jpg" height="401" width="300" alt="">
	<button>淡入</button>
	<button>淡出</button>
	<button>切换</button>
	<hr/>
	<h2>如何做一名优秀的程序员</h2>
	<div>
		<p>专业的的理论教程，系统的知识学习面 要成为一名出色的程序员，从数据结构、算法。数据库都需要系统全面的了解和认识，并可以灵活运用。对自己所从事的编程语言要灵活调用。
		</p>
		<p>
			不断尝试，乐于挑战 编程高手都是从不断的失败和尝试中走出来的，所以对于一个刚入门的新手来说，任务就是不断的去编程，发现自身存在的缺陷，以及更熟练的掌握各种数据接口的调试和数据调用的应用。
		</p>
		<p>
			好学，不耻下问 成功都是建立在无数次尝试的基础上的，同时也需要利用前辈们已经得出的一些规律，尽量的少走弯路
		</p>
		保持良好的心态 编程每天对着的都是一些枯燥的单词以及数据，所以保持一个良好的心态是至关重要的，只有拥有一个良好的心态，才是端正自己学习和勤奋的根本
		<p>
			善于从生活中发现需求 每一个程序都是为了满足网名的一种需求，所以发现网名的的需求，并把这种需求利用程序解决，可以极大的促进自己的职业发展
		</p>
		<p>
			扩大自己的视野 编程的同时，我们也要紧跟时代的步伐，学习更多的前进的经验以及技术，更好的为自己所用。
		</p>
	</div>
	<hr/>
	<button id="left">«</button>
	<button id="right">»</button>
	<div class="block" style="border: 1px solid red; width: 100px; height: 100px; position: relative;"></div>
	<script type="text/javascript">
	$(function() {
		/*淡入*/
		$("button:eq(0)").click(function() {
			//$("img").fadeIn(2000);
			$("img").fadeTo("slow", 0.5)
		});
		/*淡出*/
		$("button:eq(1)").click(function() {
			$("img").fadeOut(2000);
		});          	
		$("button:eq(2)").click(function() {
			$("img").fadeToggle(2000);
		});
		//滑上滑下
		$("h2").click(function() {
			$(this).next().slideToggle(5000);
		});
		/*自定义动画*/
		$("#right").click(function() {
			$("div[class='block']").animate({ left: "+=100px", top: "+=100px" }, 2500);
		});
		$("#left").click(function() {
			$("div[class='block']").animate({ left: "0", top: "0" }, 5000);
		});
	});
	</script>
</body>
```

## 9. jQuery的 DOM操作

- DOM(Document Object Model : 文档对象模型)
- DOM操作的分类
  - DOM Core: 并不专属于JavaScript，任何一种支持DOM的程序设计语言都可以使用，它的用途并非仅限处理网页，也可以用来处理任何一种用标记语言写出的文档，例如xml
  - HTML-DOM:使用JavaScript和DOM为HTML文件编写脚本时，有许多专门HTML-DOM属性
  - CSS-DOM:针对CSS操作，在JavaScript中,CSS-DOM主要用于获取和设置style对象的各种属性

### 9.1.属性操作

- 常用方法

  - attr('属性名',‘属性值’);
  - removeAttr('属性名');      //把属性直接删除
  - prop('属性名','属性值')
  - removeProp('属性名'); //3.2.1没成功

  ```html
  <body>
  	<img/>
  	<br/>
  	<button>添加图片</button>
  	<button>移除图片</button>
  	<br/>
  	<button>添加图片2</button>
  	<button>移除图片2</button>
  	<script type="text/javascript">
  		$(function() {
  			/*添加图片*/
  			$("button:eq(0)").click(function() {
  				//var imgNodes=document.getElementsByTagName("img");
  				//imgNodes[0].setAttribute("src","img/zbz.jpg");
  				$("img").attr("src", "img/zbz.jpg");
  			});
  			$("button:eq(2)").click(function() {
  				$("img").prop("src", "img/zbz.jpg");
  			});
  			/*删除图片*/
  			$("button:eq(1)").click(function() {
  				$("img").removeAttr("src");
  			});
  			/*删除不了，BUG*/
  			$("button:eq(3)").click(function(event) {
  				 $("img").removeProp("src");
  			});
  		});
  	</script>
  </body>
  ```

### 9.2. 操作样式

- css('样式名','样式值');
- addClass()  加样式 
- removeClass() 删除样式 
- toggleClass() 切换样式 
- hasClass() 是否包含此样式

```html
<body>
	<img src="img/zbz.jpg" class="class01 class02" height="401" width="300" alt="">
	<br/>
	<button>添加样式</button>
	<button>切换</button>
	<button>包含</button>
	<script type="text/javascript">
		$(function() {
			$("button:eq(0)").click(function() {
				/*$("img").css({
					"border-color": "green",
					"border-style": "solid",
					"border-width": "10px",
				});*/
				if($("img").hasClass("imgClass")) {
					$("img").removeClass("imgClass")
				} else {
					$("img").addClass("imgClass");
				}
			});
			$("button:eq(1)").click(function() {
				$("img").toggleClass("imgClass");

			});
			$("button:eq(2)").click(function() {
				var b = $("img").hasClass("imgClass");
				console.log(b);
			});
		});
	</script>
</body>
```

### 9.3. 查询内容

- text()		DOM中的innerText属性
- html()             DOM中的innerHTML属性
- val()                DOM中的value属性  

```
<body>
	<input type="text">
	<br/>
	<div>我是一个盒子</div>
	<br/>
	<div><b>我是加粗的盒子</b></div>
	<button>文本框中的值</button>
	<button>盒子中的文本</button>
	<button>盒子中的HTML内容</button>
	<script type="text/javascript">
		$(function() {
			/*获取value值*/
			$("button:eq(0)").click(function() {
				alert($("input").val());
			});
			/*获取页面text文本*/
			$("button:eq(1)").click(function() {
				alert($("div:first").text());
			});
			/*获取html标签里面的所有东西*/
			$("button:eq(2)").click(function() {
				console.log($("div:last").text());
				console.log($("div:last").html());
			});
		});
	</script>
</body>
```

### 9.4. 节点操作（重点留到国庆后）

#### 9.4.1. 插入节点

- 动态创建 HTML 元素并没有实际用处, 还需要将新创建的节点插入到文档中, 即成为文档中某个节点的子节点
  - 内部操作
    - append(内容) 			  末尾追加内容
      - appendTo(标签) 	         追加到(标签)
    - prepend(内容)                         开始处追加内容      
    - prependTo(标签)                    追加到(标签)
  - 外部操作
    - after(内容)                                       后边
    - before(内容)                                  前边
    - insertAfter(标签)                            插入到之后  
    - insertBefore(标签)                        插入到之前

以上方法不但能将新创建的 DOM 元素插入到文档中, 也能对原有的 DOM 元素进行移动.

#### 9.4.2. 删除节点

- remove()：删除元素本身和所有子元素内容，事件也删除，返回删除后的jQuery对象
- detach():    删除元素本身和所有子元素内容，事件不删除，返回删除后的jQuery对象
- empty()：清空节点 – 清空元素中的所有后代节点(不包含属性节点).

#### 9.4.3. 修改节点

- 复制节点 
  - clone(): 克隆匹配的 DOM 元素, 返回值为克隆后的副本. 但此时复制的新节点不具有任何行为.
  - clone(true): 复制元素的同时也复制元素中的的事件 
- 替换节点 
  - replaceWith(): 将所有匹配的元素都替换为指定的 HTML 或 DOM 元素
  - replaceAll(): 颠倒了的 replaceWith() 方法.
    注意: 若在替换之前, 已经在元素上绑定了事件, 替换后原先绑定的事件会与原先的元素一起消失


- 包裹节点 
  - wrap(): 将指定节点用其他标记包裹起来. 该方法对于需要在文档中插入额外的结构化标记非常有用, 而且不会破坏原始文档的语义.
  - wrapAll(): 将所有匹配的元素用一个元素来包裹. 而 wrap() 方法是将所有的元素进行单独包裹.
  - wrapInner(): 将每一个匹配的元素的子内容(包括文本节点)用其他结构化标记包裹起来.

```html
<body>
	<h2>热门动画排行</h2>
	<ul>
		<!--preprnd()-->
		<li>名侦探柯南</li>
		<li>七龙珠</li>
		<li>海贼王</li>
		<!--append-->
	</ul>
	<hr/>
	<button>empty</button>
	<button>remove</button>
	<button>detach</button>
	<hr/>
	<button>普通copy</button>
	<hr/>
	<button>replaceWith</button>
	<button>replaceAll</button>
	<hr/>
	<button>wrap</button>
	<button>wrapAll</button>
	<button>wrapInner</button>
	<button>unwrap</button>
	<script type="text/javascript">
		$(function() {
			/*创建jquery对象方法1*/
			var $li01 = $("<li class='class01'>奥特曼</li>");

			/*追加后面*/
			$("ul").append($li01); /*创建jquery对象方法2*/
			var $li02 = $("<li>", {
				class: "class02",
				text: "天线宝宝",
				style: "background-color:#ccc"
			});
			$li02.appendTo("ul");
			/*追加前面*/
			var $li05 = $("<li class='class01'>名侦探柯南前传</li>");
			//$("ul").prepend($li05);
			$li05.prependTo("ul");

			var $li03 = $("<li class='class01'>九妹</li>");
			var $li04 = $("<li class='class01'>六神花露水</li>");
			//$("ul>li:eq(2)").after($li03);
			//$("ul>li:eq(2").before($li03);
			$li03.insertAfter("ul>li:eq(2)");
			$li04.insertBefore("ul>li:eq(2)");
			/*删除元素里的内容*/
			$("button:eq(0)").click(function() {
				$("li:eq(2)").empty();
			});
			$("button:eq(1)").click(function() {
				/*获取第三个对象*/
				var $li03 = $("li:eq(2)");
				/*给第三个对象绑定一个点击事件*/
				$li03.bind("click", function() {
					alert("我是第三个");
				});
				$li03.remove();
				$("ul").append($li03);
			});
			$("button:eq(2)").click(function() {
				/*获取第三个对象*/
				var $li03 = $("li:eq(2)");
				/*给第三个对象绑定一个点击事件*/
				$li03.bind("click", function() {
					alert("我是第三个");
				});
				$li03.detach();
				$("ul").append($li03);
			});
			/*clone*/
			$("button:eq(3)").click(function() {
				var $lastLi = $("li:last").clone();
				$("ul").append($lastLi);
			});
			/*替换*/
			$("button:eq(4)").click(function() {
				$("li:eq(2)").replaceWith("<li>火影</li>");
			});
			$("button:eq(5)").click(function() {
				$("<li>火影</li>").replaceAll($("li:eq(2)"));
			});
			/*把每一个li都加一个包裹*/
			$("button:eq(6)").click(function() {
				var $p = $("<p>");
				$("li").wrap($p)
			});
			/*把所有li加在一个包裹里面*/
			$("button:eq(7)").click(function() {
				var $p = $("<p>");
				$("li").wrapAll($p)
			});
			/*给每一个li里面的东西加一个包裹*/
			$("button:eq(8)").click(function() {
				var $p = $("<p>");
				$("li").wrapInner($p)
			});
			/*移除父类这个没用的包裹*/
			$("button:eq(9)").click(function() {
				$("li").unwrap()
			});
		});
	</script>
</body>
```

### 9.5. 节点遍历

- 遍历子元素
  - children()   儿子们
- 遍历同辈元素
  - next()	下一兄弟
  - prev()      上一兄弟
  - nextAll()   下所有兄弟
  - prevAll()   上所有兄弟
  - siblings()   两边兄弟
- 前辈
  - parent()     父元素
  - parents()   父元素们

```html
<body>
	<div>aaaaaaaaaaaaaa</div>
	<ul>
		<li>08</li>
		<li>09</li>
		<li>10</li>
		<li id="item">11</li>
		<li>12</li>
		<li>13</li>
		<li>14</li>
		<ul>
			<li id="item01">116</li>
		</ul>
	</ul>
	<script type="text/javascript">
		$(document).ready(function() {
			/*ul下的所有的li*/
			$("ul").children("li").css("background-color", "#ccc");

			/*eq()名字记不住不要紧，要记住她*/
			$("ul").children("li").eq(1).css("text-indent", "2em");

			/*下一个*/
			$("#item").next().css("background-color", "#f40");
			//$("#item+li").css("background-color", "#f40");
			/*下面所有*/
			$("#item").nextAll().css("background-color", "#f40");
			/*前一个*/
			$("#item").prev().css("font-size", "30px");
			/*前面所有*/
			$("#item").prevAll().css("font-size", "30px");
			/*兄弟姐妹*/
			$("#item").siblings().css("color", "blue");

			/*前辈*/
			$("#item01").parent().css("list-style", "none");
			$("#item01").parents("ul").css("list-style", "none");
		});
	</script>
</body>
```

## 9.6. css-dom

- css()
- height()
- width()
- innerHeight()
- innerWidth()
- outterHeight()
- outterWidth()
- offset()

## 10. 表单验证

-  作用一：减轻服务器负担
-  作用二：户录入数据的有效性

常用验证

- 非空验证
- 长度验证
- 邮箱验证
- 手机、电话验证
- 生日验证
- 身份证验证
- 区号验证
- 中文验证

```
<body>
	<form action="success.html" method="get">
		<table border="0" cellspacing="0" cellpadding="0">
			<tr>
				<td><label>用户名</label></td>
				<td><input type="text" name="username" /></td>
			</tr>
			<tr>
				<td><label>密码</label></td>
				<td> <input type="password" name="pwd" /></td>
			</tr>
			<tr>
				<td><label>确认密码</label></td>
				<td> <input type="password" name="repwd" /></td>
			</tr>
			<tr>
				<td><label>手机号码</label></td>
				<td><input type="text" name="phone" /></td>
			</tr>
			<tr>
				<td colspan="2" align="center"><input type="submit" value="立即注册" /><br /></td>
			</tr>
		</table>
	</form>
	<script type="text/javascript">
		$("input[type='submit']").click(function() {
			var reg = /^1(3|4|5|7|8)\d{9}$/;
			var username = $("input[name='username']").val();
			if(username == undefined || username == "" || username == null) {
				var $span = $("<span>", {
					style: "color:red",
					text: "用户名不能为空"
				});
				$("input[name='username']").after($span);
				return false;
			}
			$("form").submit();
			return false;
		});
	</script>
</body>
```

## 11. 相关函数 data()、each()、serialize()

### 11.1. data在元素上存放数据

- data(key,value)
- removeData(key)

### 11.2. each() 通用例遍方法，可用于例遍对象和数组

### 11.3. serialize() 序列化表格内容，一般用于ajax请求

```html
<body>
	<form action="success.html" method="get">
		<table border="0" cellspacing="0" cellpadding="0">
			<tr>
				<td><label>用户名</label></td>
				<td><input type="text" name="username" /></td>
			</tr>
			<tr>
				<td><label>密码</label></td>
				<td> <input type="password" name="pwd" /></td>
			</tr>
			<tr>
				<td><label>确认密码</label></td>
				<td> <input type="password" name="repwd" /></td>
			</tr>
			<tr>
				<td><label>手机号码</label></td>
				<td><input type="text" name="phone" /></td>
			</tr>
			<tr>
				<td colspan="2" align="center"><input type="submit" value="立即注册02" /><br /></td>
			</tr>
		</table>
	</form>
	<button id="eachId">each</button>
	<button id="dataId">data</button>
	<button id="serializeId">serialize</button>
	<script type="text/javascript">
		$(function() {
			/*遍历*/
			$("#eachId").click(function() {
				var $inputs = $("input");
				$.each($inputs, function(i, e) {
					console.log(i + e.value)
				});
				$inputs.each(function(i) {
					console.log($(this).val())
				});
			});
			/*data在元素上存放一个数据key-value;或者根据key取值*/
			$("#dataId").click(function() {
				var $username = $("input[name='username']");
				$username.data({ key01: "value01", key02: "value02" });
				console.log($username);
				//removeData，移除数据（key）
				//$("input[name='username']").removeData("key01")
				alert($("input[name='username']").data("key01"));
				alert($("input[name='username']").data("key02"));
			});
			/*serialize()序列化表单数据*/
			$("#serializeId").click(function() {
				console.log($("form").serialize());
			});
		});
	</script>
</body>
```

### 













