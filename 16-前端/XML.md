# XML

# 1. 概念

## 1.1. ML(Markup Language)标记语言

- 标记语言:都有<标记开始><标记结束/>
- 当下流行的两种:HTML(Hyper Text Markup Language )和XML(eXtensible Markup Language)
- HTML和XML都是由同种父语言SGML(Standard Generalized Markup Language)发展起来的
- HTML和XML都是W3C制定规则的

## 1.2. HTML:标记是固定的

- 功能 
  - 表单验证
  - 页面特效
- 关注是数据展示和用户体验（显示数据）
- 标记是固定的，不可扩展

## 1.3. XML:标记是扩展的

- 功能 
  - 传输数据（类似json）
  - 项目配置文件（web.xml）
- 关注是数据本身和数组结构（传输数据）
- 标记是自定义的，可扩展的

## 1.4.HTML与XML对比

- 相同点
  - 都是标记语言，标签都有开始和结束标签
  - 都是SGML基础上发起来的
  - 都是由W3C负责制定规范的
- 不同点:
  - XML不是HTML的替代，它们为不同目的而设计的
  - HTML重点是来显示数据，而XML重点是来传输数据和项目配置文件
  - HTML的标记是固定的不可扩展，而ML标记是自定义的可以扩展

## 1.5. 解析器

- 专用工具 XML Spy
- 浏览器
- Sublime
- Eclipse

##1.6. 示例对比

- HTML示例

  ```html
  <!DOCTYPE html>
  <html lang="zh_CN">

  <head>
      <meta charset="UTF-8">
      <title>Document</title>
  </head>

  <body>
      <table border="1" width="60%" align="center">
          <tr>
              <th>
                  学号
              </th>
              <th>
                  姓名
              </th>
          </tr>
          <tr>
              <td>
                  1001
              </td>
              <td>
                  张三
              </td>
          </tr>
          <tr>
              <td>
                  1002
              </td>
              <td>
                  李四
              </td>
          </tr>
      </table>
  </body>
  </html>
  ```

- XML示例

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <students>
  	<student>
  		<id>1001</id>
  		<name>张三</name>
  		<age>18</age>
  		<gender>男</gender>
  		<address>南京</address>
  	</student>
  	
  	<student>
  		<id>1002</id>
  		<name>李四</name>
  		<age>19</age>
  		<gender>女</gender>
  		<address>苏州</address>
  	</student>
  </students>
  ```

# 2. XML语法规则

- 第一行为版本和编码的声明 ，

  ```xml
  <?xml version="" encoding="UTF-8"?>
  ```

  - 一般不省略
  - "?"的两边都没有空格，注：虽最后一个"?"的左边可以有空格
  - 有两个属性名version,encoding, 属性与属性之间用空格隔开，属性要包含属性名和属性值，属性名和值之间用的"=",属性值一般还需要加上双引号

- 内容

  - 都必须有开有关
  - 嵌套要正确
  - 根元素有且只有一个
  - XML区分大小写
  - XML中，空格会被保留，而HTML会把连续多个空格合并成一个
    - XML中，空格会被保留，跟HTML一样，会把连续多个空格合并成一个（参考）

# 3. XML组成部分

## 3.1. XML元素

###3.1.1.  定义

XML元素是指从 开始标签(左尖括号)~结束标签(右尖括号)中的所有内容。

可以包含:

- 其它元素
- 文本

```xml
<students>
	<student>
		<id>1001</id>
		<name>张   三</name>
		<age>18</age>
		<gender>男</gender>
		<address>南京</address>
	</student>
	
	<student>
		<id>1002</id>
		<name>李四</name>
		<age>19</age>
		<gender>女</gender>
		<address>苏州</address>
	</student>
</students>
```

### 3.1.2.  元素命名规则

- 可以字母、数字、下划线、点、中划线和冒号
- 开头只能是字母和下划线
- 名称中不能空格和其它特殊符号
- 可以使用任何名称，没有保留字

### 3.1.3. 最佳命名习惯

- 见名识义
- 名称应当比较简短
- 避免"-"、"."、":"

## 3.2. XML属性 

- XML元素可以在标签中包含属性

###3.2.1. XML属性 

在HTML中，IMG元素代码 `<img src="xxx.jpg"/><a href="xxx.html">xxx</a> `

- 属性是提供元素的额外信息

- 属性通常用担任不属于数据组成部分的信息。下面例子，文件类型与数据无数。但在解析元素时属性内容就至关重要了

  ```html
  <file type="jpeg">computer.jpg</file>
  ```

###3.2.2. XML的属性值

- 必须加引号，等于号两边不要加上空格 


- 属性值必须补引号包围，不过单引号和双引号均可。

  - 比如一个学生的性别

    ```xml
    <student gender='male'></student>
    或
    <student gender="maile"></student>
    ```

    注:如果值本身就是双引号，那么属性的值就用单引号

    ```xml
    <student name='张"三"' ></student>
    ```

### 3.2.3. XML元素和属性

```xml
 <person sex="female">
      <firstname>Anna</firstname>
      <lastname>Smith</lastname>
  </person>
  <person>
      <sex>female</sex>
      <firstname>Anna</firstname>
      <lastname>Smith</lastname>
  </person>
```

在第一个person中的sex用属性表示，第二个person中的sex用元素表示。

两者都提供相似的信息。

- 什么时候用属性，什么时候用元素？
  - 在XML中优先考虑使用元素。

## 3.3. 实体引用 5个 

```html
<			&lt;	
>			&gt;
'			&apos;
"			&quot;
&			&amp;
```

## 3.4. XML CDATA 

在CDATA区域的内容都会被XML解析器作纯文本输出，例如<和&都不需要解析了，直接输出即可

语法

<![CATA[

​	内容

]]>

```xml
<student>
		<id>1002</id>
		<name>李四</name>
		<age>19</age>
		<gender>女</gender>
		<!-- 学生地址 -->
		<address>
			苏州 &lt; &gt;&amp;&apos;&quot;
			<![CDATA[
				<xxx> xxx </xx>
				&
			]]>
		 </address>
	</student>
```



## 3.5. 注释 

- XML和HTML相同
  - <!-- 注释内容 -->
- 编译时是忽略的，但仍然传递到页面代码中

# 4. 使用CSS显示XML

- 通过使用css，可为XML文档添加显示信息

- 步骤

  1. 新建base.css文件，写入样式

  ```xml
  *{
  display:block;
  }

  id{
  	color:red;
  	font-size:24px;
  }
  ```

  1. 在*.xml中，引入base.css,xml的内容即可按照样式表效果呈现

  ```xml
  <?xml-stylesheet type="text/css" href="base.css"?>
  ```

- 示例代码 

  ```html
  <?xml    version="1.0" encoding="UTF-8" ?>
  <?xml-stylesheet type="text/css" href="base.css"?>
  <students>
  	<student>
  		<id>1001</id>
  		<name>张   三</name>
  		<age>18</age>
  		<gender>男</gender>
  		<address>南京</address>
  	</student>
  	
  	<student>
  		<id>1002</id>
  		<name>李四</name>
  		<age>19</age>
  		<gender>女</gender>
  		<address>
  			苏州
  		 </address>
  	</student>

  </students>
  ```

#5. 使用XSLT显示XML

- XSLT是首选的XML的样式表语言

- XSLT(eXtensible StyleSheet Language Transformations)，XSLT要比CSS更加完善

- 使用XSLT是在浏览器显示 XML文件之前，先把它转换为HTML

- 实现步骤

  - 新建.xslt

  ```html
  <?xml version="1.0" encoding="UTF-8"?>
  <html xsl:version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns="http://www.w3.org/1999/xhtml">
  	<head>
  		<title>XSLT的使用</title>
  		<style>
  			ul{
  			list-style:none;
  			}
  			ul>li:first-child{
  				color:red;
  				font-size:20px;
  			}
  		</style>	
  	</head>
  	<body style="background-color:#ccc;">
  		<xsl:for-each select="students/student">
  			<ul>
  				<li>
  					<xsl:value-of select="id"/>	
  				</li>
  				<li>
  					<xsl:value-of select="name"/>	
  				</li>
  				<li>
  					<xsl:value-of select="age"/>	
  				</li>
  				<li>
  					<xsl:value-of select="gender"/>	
  				</li>
  				<li>
  					<xsl:value-of select="address"/>	
  				</li>
  			</ul>
  		</xsl:for-each>
  	</body>	
  </html>
  ```

  - 在XML中引用XSL

    ```html
    <?xml-stylesheet type="text/xsl" href="students.xslt"?>
    ```

  - HTML代码

  ```html
  <?xml    version="1.0" encoding="UTF-8" ?>
  <!-- ?xml-stylesheet type="text/css" href="base.css"? -->
  <?xml-stylesheet type="text/xsl" href="students.xslt"?>
  <students>
  	<student>
  		<id>1001</id>
  		<name>张   三</name>
  		<age>18</age>
  		<gender>男</gender>
  		<address>南京</address>
  	</student>
  	
  	<student>
  		<id>1002</id>
  		<name>李四</name>
  		<age>19</age>
  		<gender>女</gender>
  		<address>
  			苏州
  		 </address>
  	</student>
  </students>
  ```

# 6. XML验证

- 拥有正确语法的XML被称为"格式良好的"XML，再通过DTD或Schema验证的XML被称为"验证通过的"XML

- 合法的XML=格式良好的XML+验证通过的XML

- 规范由来

  - 示例：下面两段XML，数据量是相同，但结构不一样，无法进行数据交互的

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <计算机书籍>
      <书名>java</书名>
      <价格>50</价格>
      <简介>Havaing in Java</简介>
  </计算机书籍>
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <book>
    	<name>java</name>
      <price>50</price>
      <info>Havaing in Java</info>
  </book>
  ```

- 形式良好的XML文档

  - 语法要正确
  - 必须有根元素
  - 必须要关闭
  - 大小敏感
  - 正确嵌套
  - 属性值加上引号

- 验证通过

  - DTD
  - Schema来规范XML的标记规则

##6.1. DTD验证

- 定义： DTD(Document Type Definition)：文档类型定义

- DTD使用用定义XML约束格式，约束XML文件中标记语言

- DTD类型

  - SYSTEM  (小范围定义)
  - PUBLIC  (行业共用)

###6.1.1. DTD 外部验证

- 步骤

  - 定义DTD文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!ELEMENT students (student*)>
  <!ELEMENT student (id,name,age,gender*,address)>
  <!ELEMENT id (#PCDATA)>
  <!ELEMENT name (#PCDATA)>
  <!ELEMENT age (#PCDATA)>
  <!ELEMENT gender (#PCDATA)>
  <!ELEMENT address (#PCDATA)>

  * 代表可有可无可多个
  + 至少一个
  ? 最多一个
    有且只有一个
    (#PCDATA)元素里放置是文本内容 
  元素名  与 后面()中的描述必须加上空格 
  ```

  - 在XML引用DTD

  ```xml
  <!DOCTYPE 根元素名 SYSTEM|PUBLIC "dtd文件名">
  ```

  - XML代码

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE students SYSTEM "students.dtd">
  <students>
  	<student>
  		<id>1001</id>
  		<name>张   三</name>
  		<age>18</age>
  		<gender>男</gender>
  		<address>南京</address>
  	</student>
  	
  	<student>
  		<id>1002</id>
  		<name>李四</name>
  		<age>19</age>
  		<gender>女</gender>
  		<address>
  			苏州
  		 </address>
  	</student>
  </students>
  ```

###6.1.2. DTD 内部验证

- 需要把DTD内容放置在

  <!DOCTYPE 根元素名 [

  ​	

  ]>

- 示例

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <!DOCTYPE BOOK [
  	
  <!ELEMENT BOOK (TITLE, AUTHOR+, INTRODUCTION?,CHAPTER+)>
  <!ELEMENT AUTHOR (LASTNAME?, FIRSTNAME?)>
  <!ATTLIST BOOK ISBN CDATA #REQUIRED>
  <!ATTLIST AUTHOR ID CDATA #IMPLIED>
  <!ELEMENT TITLE (#PCDATA)>
  <!ELEMENT INTRODUCTION (TITLE, TEXT?)>
  <!ELEMENT CHAPTER (NUM, TITLE, TEXT+)>
  <!ELEMENT NUM (#PCDATA)>
  <!ELEMENT TEXT (#PCDATA)>
  <!ELEMENT LASTNAME (#PCDATA)>
  <!ELEMENT FIRSTNAME (#PCDATA)>
  ]>
  ```


  <BOOK ISBN="1001">
  	<TITLE></TITLE>
  	<AUTHOR ID="三毛">
  		<LASTNAME></LASTNAME>	
  		<FIRSTNAME></FIRSTNAME>
  	</AUTHOR>
  	<INTRODUCTION>
  		<TITLE></TITLE>
  		<TEXT></TEXT>
  	</INTRODUCTION>
  	<CHAPTER>
  		<NUM></NUM>
  		<TITLE></TITLE>
  		<TEXT></TEXT>
  	</CHAPTER>
  </BOOK>

## 6.2.  Schema验证

- 特点   
  - 加上了命名空间，避免标记命名冲突  
  - 对元素的验证更丰富- 示例


- 示例

  ```
  <table>
    <tr>
      <td>这是表格行中的数据</td>
    </tr>
  </table> 
  ```

```xml
  <table>
    <type>长桌</type>
    <meterial>木头</meterial>
  </table> 
```

- 用命名空间就可以解决

  ```xml
  <html:table>
    <tr>
      <td>这是表格行中的数据</td>
    </tr>
  </html:table> 
  ```

  ```xml
  <product:table>
    <type>长桌</type>
    <meterial>木头</meterial>
  </product:table> 
  ```

### 6.2.1. Schema与DTD的比较

相同点：

​	能名验证XML文档

​	都使用XML语法实现

​	都符号W3C的规范

不同点：

​	Schema是DTD的替换者

​	Schema可以命名空间，而DTD不可以

​	Schema验证更丰富，而DTD验证较少

​	Schema扩展名.xsd，而DTD扩展名为.dtd

xsd：XML Schema Definition

### 6.2.2. Schema入门示例

- 定义students.xsd文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
targetNamespace="http://www.w3school.com.cn"
xmlns="http://www.w3school.com.cn"
elementFormDefault="qualified">
  <xs:element name="students">
      <xs:complexType>
          <xs:sequence>
              <xs:element name="student" minOccurs="1" maxOccurs="5">
                  <xs:complexType>
                      <xs:sequence>
                          <xs:element name="id" type="xs:integer"></xs:element>
                          <xs:element name="name">
                        	  <xs:complexType>
                                <xs:all>
                                	<xs:element name="lastame" type="xs:string"></xs:element>
                                    <xs:element name="firstname" type="xs:string"></xs:element>
                                </xs:all>
                              </xs:complexType>
                        	</xs:element>
                          <xs:element name="age" type="xs:integer"></xs:element>
                          <xs:element name="gender" type="xs:string"></xs:element>
                          <xs:element name="address" type="xs:string"></xs:element>
                      </xs:sequence>
                  </xs:complexType>
              </xs:element>
          </xs:sequence>
      </xs:complexType>
  </xs:element>
</xs:schema>
```

- 在xml中引用students.xsd文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<school:students xmlns:school="http://www.w3school.com.cn" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.w3school.com.cn students.xsd">
	<school:student>
		<school:id>1001</school:id>
		<school:name></school:name>
		<school:age>18</school:age>
		<school:gender></school:gender>
		<school:address></school:address>
	</school:student>
</school:students >
```

###6.2.3.  组成 和 验证

#### 6.2.3.1. 简易元素

- 语法 

  ```xml
  <xs:element name="xxx" type="yyy">
  ```

  name定义元素名称 ，type定义元素的数据类型，数据类型是Schema内置的

  常用数据类型如下:

  - xs:string
  - xs:integer
  - xs:decmial
  - xs:float
  - xs:double
  - xs:boolean
  - xs:date
  - xs:time
  - xs:dateTime

- 示例

  ```xml
  <lastname>Smith</lastname>
  <age>28</age>
  <dateborn>1980-03-27</dateborn>
  ```

  ```xml
  <xs:element name="lastname" type="xs:string"></xs:element>
  <xs:element name="age" type="xs:integer"></xs:element>
  <xs:element name="dateborn" type="xs:date"></xs:element>
  ```

- 简易元素的默认值和固定值(不能改变)

  ```xml
  <xs:element name="lastname" type="xs:string" fixed="smith"></xs:element>
  <xs:element name="age" type="xs:integer" default="18"></xs:element>
  <xs:element name="dateborn" type="xs:date"></xs:element>
  ```

#### 6.2.3.2. 属性

- 语法 

  ```xml
  <xs:attribute name="xxx" type="yyy"></xs:attribute>
  ```

  name定义元素名称 ，type定义元素的数据类型，数据类型是Schema内置的，类型如上

- 示例

  ```xml
  <lastname lang="en">Smith</lastname>
  ```

  ```xml
  <xs:element  name="lastname" type="xs:string" >
    <xs:complexType>
  	<xs:attribute name="lang" type="xs:string" default="en" use="required"></xs:attribute>
    </xs:complexType>
   </xs:element>
  ```

- 案例

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
  targetNamespace="http://www.w3school.com.cn"
  xmlns="http://www.w3school.com.cn"
  elementFormDefault="qualified">

  <xs:element name="students">
  	<xs:complexType>
  		<xs:sequence>
  			<xs:element name="student" minOccurs="1" maxOccurs="5">
  				<xs:complexType>
  					<xs:sequence>
  						<xs:element name="name" type="xs:string" fixed="smith"></xs:element>
  						<xs:element name="age" type="xs:integer" default="18"></xs:element>
  						<xs:element name="gender" type="xs:string"></xs:element>
  						<xs:element name="address" type="xs:string"></xs:element>
  					</xs:sequence>
  					<xs:attribute name="id" type="xs:integer" use="required"></xs:attribute>
  				</xs:complexType>
  			</xs:element>
  		</xs:sequence>
  	</xs:complexType>
  </xs:element>
  </xs:schema>
  ```

#### 6.2.3.3. 简单类型

- enumeration

  ​

- minInclusive

- maxInclusive

- minExclusive

- maxExclusive

  ​

- length

- minLength

- maxLength

- witeSpace: preserve|replace|collapse

- pattern


- 对数值范围的限定

  ```xml
  <xs:element name="age">
    <xs:simpleType>
      <xs:restriction base="xs:integer">
        <xs:minInclusive value="1"/>
        <xs:maxInclusive value="150"/>	
      </xs:restriction>
    </xs:simpleType>
  </xs:element>
  ```

- 对一组值的限定

  ```xml
  <xs:element name="car">
    <xs:simpleType>
      <xs:restriction base="xs:string">
        <xs:enumeration value="BMW"></xs:enumeration>
        <xs:enumeration value="BEN"></xs:enumeration>
        <xs:enumeration value="AUTO"></xs:enumeration>
      </xs:restriction>
    </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="[a-z]" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="[axy]" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="a|z" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="[a-ZA-Z][a-zA-Z][A-Z]" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="([a-z])*" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="([a-z])+" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:pattern value="([a-zA-Z]){8}" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:whitespace value="preserve|replace|collapse" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:length value="8" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>

  <xs:element name="letter">
      <xs:simpleType>
          <xs:restriction base="xs:string">
              <xs:minlength value="5" />
           	 <xs:maxlength value="8" />
          </xs:restriction>
      </xs:simpleType>
  </xs:element>
  ```

  ​

#### 6.2.3.4. 复合类型

- All :子元素可以按照任意顺序出现，且每个子元素必须只出现一次

  ```xml
  <xs:element name="person">
    <xs:complexType>
      <xs:all>
        <xs:element name="firstname" type="xs:string"/>
        <xs:element name="lastname" type="xs:string"/>
      </xs:all>
    </xs:complexType>
  </xs:element>
  ```

- Choice  子元素非此即彼

  ```xml
  <xs:element name="person">
    <xs:complexType>
      <xs:choice>
        <xs:element name="employee" type="employee"/>
        <xs:element name="member" type="member"/>
      </xs:choice>
    </xs:complexType>
  </xs:element>
  ```

- Sequence :子元素必须按照特定的顺序出现

  ```xml
  <xs:element name="person">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="firstname" type="xs:string"/>
        <xs:element name="lastname" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  ```

- minOccurs  元素最少出现次数

- maxOccurs 元素最多出现次数

#7. 解析XML

##7.1 四种方式

现在解析XML 主流的方式有四种,即DOM、SAX、JDOM和DOM4J

后面两种是专门针对于Java中的DOM进行操作的

使用不同操作方式需要其对应的jar包

DOM：在现在的Java JDK里都自带了，在xml-apis.jar包里

SAX：<http://sourceforge.net/projects/sax/>

JDOM：<http://jdom.org/downloads/index.html>

DOM4J：<http://sourceforge.net/projects/dom4j/>

要解析xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<users>
    <user id="0">
        <name>Alexia</name>
        <age>23</age>
        <sex>Female</sex>
    </user>
    <user id="1">
        <name>Edward</name>
        <age>24</age>
        <sex>Male</sex>
    </user>
    <user id="2">
        <name>wjm</name>
        <age>23</age>
        <sex>Female</sex>
    </user>
    <user id="3">
        <name>wh</name>
        <age>24</age>
        <sex>Male</sex>
    </user>
</users>
```

## 7.2. DOM(Document Object Model) 

- DOM是与平台和语言无关的方式表示XML文档的结构，DOM是节点层次结构集合，DOM被认为是基于树的，DOM需要在整个文档和结构加载完成之后才能操作。
- 优点
  - 可以对数据和结构进行修改
- 缺点
  - 需要加载整个XML文档，然后才进解析操作，消耗资源较大

```java
package dom;
import java.io.IOException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

public class DOMTest {
    public static void main(String[] args) {
        //创建一个DocumentBuilderFactory的对象
        DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
        //创建一个DocumentBuilder的对象
        try {
            //创建DocumentBuilder对象
            DocumentBuilder db = dbf.newDocumentBuilder();
            //通过DocumentBuilder对象的parser方法加载books.xml文件到当前项目下
            Document document = db.parse("books.xml");
            //获取所有book节点的集合
            NodeList bookList = document.getElementsByTagName("book");
            //通过nodelist的getLength()方法可以获取bookList的长度
            System.out.println("一共有" + bookList.getLength() + "本书");
            //遍历每一个book节点
            for (int i = 0; i < bookList.getLength(); i++) {
                System.out.println("=================下面开始遍历第" + (i + 1) + "本书的内容=================");
                //通过 item(i)方法 获取一个book节点，nodelist的索引值从0开始
                Node book = bookList.item(i);
                //获取book节点的所有属性集合
                NamedNodeMap attrs = book.getAttributes();
                System.out.println("第 " + (i + 1) + "本书共有" + attrs.getLength() + "个属性");
                //遍历book的属性
                for (int j = 0; j < attrs.getLength(); j++) {
                    //通过item(index)方法获取book节点的某一个属性
                    Node attr = attrs.item(j);
                    //获取属性名
                    System.out.print("属性名：" + attr.getNodeName());
                    //获取属性值
                    System.out.println("--属性值" + attr.getNodeValue());
                }
                //解析book节点的子节点
                NodeList childNodes = book.getChildNodes();
                //遍历childNodes获取每个节点的节点名和节点值
                System.out.println("第" + (i+1) + "本书共有" + 
                childNodes.getLength() + "个子节点");
                for (int k = 0; k < childNodes.getLength(); k++) {
                    //区分出text类型的node以及element类型的node
                    if (childNodes.item(k).getNodeType() == Node.ELEMENT_NODE) {
                        //获取了element类型节点的节点名
                        System.out.print("第" + (k + 1) + "个节点的节点名：" 
                        + childNodes.item(k).getNodeName());
                        //获取了element类型节点的节点值
                        System.out.println("--节点值是：" + childNodes.item(k).getFirstChild().getNodeValue());
                        //System.out.println("--节点值是：" + childNodes.item(k).getTextContent());
                    }
                }
                System.out.println("======================结束遍历第" + (i + 1) + "本书的内容=================");
            }
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }        
    }
}

```

增、删、改、查示例

```java
package dom;

import java.io.File;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.junit.Test;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.NamedNodeMap;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

public class DOMTest2 {

	@Test
	public void find() throws Exception {
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder builder = factory.newDocumentBuilder();
		Document document = builder.parse("students.xml");
		Element root = document.getDocumentElement();
		System.out.println("<" + root.getNodeName() + ">");
		NodeList studentNodeList = document.getElementsByTagName("student");
		for (int i = 0; i < studentNodeList.getLength(); i++) {
			Node studentNode = studentNodeList.item(i);
			System.out.print("<" + studentNode.getNodeName() + " ");
			NamedNodeMap attrs = studentNode.getAttributes();
			for (int j = 0; j < attrs.getLength(); j++) {
				Node attr = attrs.item(j);
				System.out.println(attr.getNodeName() + "=" + attr.getNodeValue());
			}
			NodeList childNodes = studentNode.getChildNodes();
			System.out.println(childNodes.getLength());
			for (int k = 0; k < childNodes.getLength(); k++) {
				if (childNodes.item(k).getNodeType() == Node.ELEMENT_NODE) {
					System.out.println("\t<" + childNodes.item(k).getNodeName() + ">"
							+ childNodes.item(k).getFirstChild().getNodeValue());
				}
			}

		}
	}

	@Test
	public void update() throws Exception {
		// 创建文件工厂实例
		DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
		dbf.setIgnoringElementContentWhitespace(true);
		try {
			// 从XML文档中获取DOM文档实例
			DocumentBuilder db = dbf.newDocumentBuilder();
			// 获取Document对象
			Document xmldoc = db.parse("students.xml");

			
			 XPathFactory factory = XPathFactory.newInstance(); //创建 XPathFactory  
             XPath xpath = factory.newXPath();//用这个工厂创建 XPath 对象  
              
             NodeList nodes = (NodeList)xpath.evaluate("students/student[@id='1001']", xmldoc, XPathConstants.NODESET);  
             Element e = (Element) nodes.item(0);
			// 将age节点的内容更改为18
			e.getElementsByTagName("age").item(0).setTextContent("18");
			// 保存
			TransformerFactory tfactory = TransformerFactory.newInstance();
			Transformer former = tfactory.newTransformer();
			former.transform(new DOMSource(xmldoc), new StreamResult(new File("students.xml")));
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	
	@Test
	public void insert() throws Exception {
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder builder = factory.newDocumentBuilder();
		Document document = builder.parse("students.xml");
		Element root = document.getDocumentElement();
		
        Element studentElement = document.createElement("student");
        studentElement.setAttribute("id", "1003");
        // 创建节点name
        Element name = document.createElement("name");
        name.setTextContent("孙七");
        studentElement.appendChild(name);
        
        // 创建节点age
        Element age = document.createElement("age");
        age.setTextContent("0");
        studentElement.appendChild(age);
        
        // 把son添加到根节点中
        root.appendChild(studentElement);	
        
     // 保存
     			TransformerFactory tfactory = TransformerFactory.newInstance();
     			Transformer former = tfactory.newTransformer();
     			former.transform(new DOMSource(document), new StreamResult(new File("students.xml")));
     	
	}
	
	@Test
	public void delete() throws Exception {
		// 创建文件工厂实例
				DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
				dbf.setIgnoringElementContentWhitespace(true);
				try {
					// 从XML文档中获取DOM文档实例
					DocumentBuilder db = dbf.newDocumentBuilder();
					// 获取Document对象
					Document xmldoc = db.parse("students.xml");
					Element root =xmldoc.getDocumentElement();
					
					 XPathFactory factory = XPathFactory.newInstance(); //创建 XPathFactory  
		             XPath xpath = factory.newXPath();//用这个工厂创建 XPath 对象  
		              
		             NodeList nodes = (NodeList)xpath.evaluate("students/student[@id='1003']", xmldoc, XPathConstants.NODESET);  
		             Element e = (Element) nodes.item(0);
		             root.removeChild(e);
					// 保存
					TransformerFactory tfactory = TransformerFactory.newInstance();
					Transformer former = tfactory.newTransformer();
					former.transform(new DOMSource(xmldoc), new StreamResult(new File("students.xml")));
				} catch (Exception e) {
					System.out.println(e.getMessage());
				}
	}


}
```

##7.3. SAX(Simple API for XML)

- SAX 基于流的方式，能够立即执行，而不是等待所有文档都加载完成，所以SAX效率要DOM快
- 优点:
  - 不需要 等待文档加载完成，解析可立即开始
  - 在读取流的数据时检查数据，不需要保存到内存
  - 在满足解析需要后，可中断，不必须解析整个文档
  - 效率和性能高
- 缺点:
  - 需要为应用程序制定TAG处理逻辑，文档越复杂时处理逻辑就较繁琐
  - 无法访问文档的任一部分

```java
package sax;
import java.io.IOException;

import javax.xml.parsers.ParserConfigurationException;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.SAXException;

import model.Book;

public class SAXTest {
    /**
     * @param args
     */
    public static void main(String[] args) {
        SAXParserFactory factory = SAXParserFactory.newInstance();
        try {
            SAXParser parser = factory.newSAXParser();
            SAXParserHandler handler = new SAXParserHandler();
            parser.parse("books.xml", handler);
            System.out.println("~！~！~！共有" + handler.getBookList().size()
                    + "本书");
            for (Book book : handler.getBookList()) {
                System.out.println(book.getId());
                System.out.println(book.getName());
                System.out.println(book.getAuthor());
                System.out.println(book.getYear());
                System.out.println(book.getPrice());
                System.out.println(book.getLanguage());
                System.out.println("----finish----");
            }
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (SAXException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```

## 7.4. JDOM (Java-based Document Object Model)

- JDOM是为Java特别定制文档模型，它简化Java与XML的操作，比使用纯DOM来的便捷。有望成为Java标准的扩展
- JDOM面向是具体类而不是接口
- JDOM本身不包含XML解析器，它使用的是SAX2来解析的
- 优点
  - 面向实现类，简单DOM操作的API
  - 使用了大量Java集合类，Java开发人员觉得方便
- 缺点
  - 面向实现类，灵活较性
  - 性能一般

```java
package jdom;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

import org.jdom.Attribute;
import org.jdom.Document;
import org.jdom.Element;
import org.jdom.JDOMException;
import org.jdom.input.SAXBuilder;

import model.Book;

public class JDOMTest {
    private static ArrayList<Book> booksList = new ArrayList<Book>();
    /**
     * @param args
     */
    public static void main(String[] args) {
        // 进行对books.xml文件的JDOM解析
        // 准备工作
        // 1.创建一个SAXBuilder的对象
        SAXBuilder saxBuilder = new SAXBuilder();
        InputStream in;
        try {
            // 2.创建一个输入流，将xml文件加载到输入流中
            in = new FileInputStream("books.xml");
            InputStreamReader isr = new InputStreamReader(in, "UTF-8");
            // 3.通过saxBuilder的build方法，将输入流加载到saxBuilder中
            Document document = saxBuilder.build(isr);
            // 4.通过document对象获取xml文件的根节点
            Element rootElement = document.getRootElement();
            // 5.获取根节点下的子节点的List集合
            List<Element> bookList = rootElement.getChildren();
            // 继续进行解析
            for (Element book : bookList) {
                Book bookEntity = new Book();
                System.out.println("======开始解析第" + (bookList.indexOf(book) + 1)
                        + "书======");
                // 解析book的属性集合
                List<Attribute> attrList = book.getAttributes();
                // //知道节点下属性名称时，获取节点值
                // book.getAttributeValue("id");
                // 遍历attrList(针对不清楚book节点下属性的名字及数量)
                for (Attribute attr : attrList) {
                    // 获取属性名
                    String attrName = attr.getName();
                    // 获取属性值
                    String attrValue = attr.getValue();
                    System.out.println("属性名：" + attrName + "----属性值："
                            + attrValue);
                    if (attrName.equals("id")) {
                        bookEntity.setId(attrValue);
                    }
                }
                // 对book节点的子节点的节点名以及节点值的遍历
                List<Element> bookChilds = book.getChildren();
                for (Element child : bookChilds) {
                    System.out.println("节点名：" + child.getName() + "----节点值："
                            + child.getValue());
                    if (child.getName().equals("name")) {
                        bookEntity.setName(child.getValue());
                    }
                    else if (child.getName().equals("author")) {
                        bookEntity.setAuthor(child.getValue());
                    }
                    else if (child.getName().equals("year")) {
                        bookEntity.setYear(child.getValue());
                    }
                    else if (child.getName().equals("price")) {
                        bookEntity.setPrice(child.getValue());
                    }
                    else if (child.getName().equals("language")) {
                        bookEntity.setLanguage(child.getValue());
                    }
                }
                System.out.println("======结束解析第" + (bookList.indexOf(book) + 1)
                        + "书======");
                booksList.add(bookEntity);
                bookEntity = null;
                System.out.println(booksList.size());
                System.out.println(booksList.get(0).getId());
                System.out.println(booksList.get(0).getName());
                
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (JDOMException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

##7.5 DOM4j (DOM for Java)

- DOM4j最属性JDOM一个分支，后面独立出来，形成开源一个项目

- DOM4j面向的 接口和抽象，扩展时更加的灵活

- DOM4j提供了更多的API，使用更加的便捷

- DOM4j还可以集成XPath语法

- 优点

  - 大量的使用了Java集合类，Java开发人员方便
  - 支持XPath
  - 性能还可以

- 缺点

  - 大量使用了接口和更复杂的API

- 示例代码 

  ```java
  package net.wanho.xml;

  import java.io.FileOutputStream;
  import java.io.IOException;
  import java.util.Iterator;
  import java.util.List;

  import javax.xml.xpath.XPath;
  import javax.xml.xpath.XPathConstants;
  import javax.xml.xpath.XPathFactory;

  import org.dom4j.*;
  import org.dom4j.io.OutputFormat;
  import org.dom4j.io.SAXReader;
  import org.dom4j.io.XMLWriter;

  public class DOM_DOM4j {
  	Document document = null;
  	{
  		SAXReader reader = new SAXReader();
  		 try {
  			document = reader.read(DOM_DOM4j.class.getClassLoader().getResourceAsStream("students.xml"));
  		} catch (DocumentException e) {
  			e.printStackTrace();
  		}
  	}

  	public static void main(String[] args) throws Exception {
  		DOM_DOM4j demo = new DOM_DOM4j();
  		//解析所有
  		//demo.testLoad();
  		
  		//修改
  		demo.testEdit();
  	}
  	
  	
  	private void addElement(Element node,String elementName,String elementValue) throws Exception{
  		Element element =node.addElement(elementName); 
  		element.setText(elementValue);
  		saveDocument();
  	}
  	
  	public void deleteElement(Element node,String elementName) throws Exception{
  		Element elemnt = node.element(elementName);
  		node.remove(elemnt);
  		saveDocument();
  	}
  	public void addAttribute(Element node,String attrName,String attrValue) throws Exception{
  		node.addAttribute(attrName, attrValue);
  		saveDocument();
  	}
  	//删除属性
  	public void deleteAttribute(Element node,String attrName) throws Exception{
  		Attribute attr=node.attribute(attrName);
  		if(attr!=null)
  			node.remove(attr);
  		saveDocument();
  	}
  	public void saveDocument() throws Exception{
  		XMLWriter writer = new XMLWriter(new FileOutputStream("src/students.xml"),OutputFormat.createPrettyPrint());
  		writer.write(document);
  		writer.close();
  	}
  	
  	
  	private void testEdit() throws Exception {
          
          
          Element studentNode =   (Element) 	document.selectSingleNode("/students/student[@id='1003']");
          
  		addAttribute(studentNode, "hm", "nj");
  		
  		//加属性
  		//node.addAttribute("home", "nj");
  		
  		//Attribute attr=node.attribute("id");
  		//删除属性
  		//node.remove(attr);
  		
  		//加元素
  		//Element element =node.addElement("birth"); 
  		//element.setText("1990");
  		//删元素
  		/*Element elemnt = node.element("birth");
  		node.remove(elemnt);*/
  		
  //		//保存
  //		XMLWriter writer = new XMLWriter(new FileOutputStream("src/students.xml"),OutputFormat.createPrettyPrint());
  //		writer.write(document);
  //		writer.close();
  	}

  	private static void testLoad() throws DocumentException {
  		SAXReader reader = new SAXReader();
  		Document document = reader.read(DOM_DOM4j.class.getClassLoader().getResourceAsStream("students.xml"));
  		Element root = document.getRootElement();
  		System.out.println(root.getName());
  		
  		Iterator<Element> es = root.elementIterator();
  		while(es.hasNext()){
  			Element e = es.next();
  			System.out.print(e.getName()+"\t");
  			
  			List<Attribute> as= e.attributes(); 
  			for(Attribute a:as){
  				System.out.println(a.getName()+"="+a.getValue());
  			}
  			
  			List<Element> es2 = e.elements();
  			for(Element ee:es2){
  				System.out.println("\t"+ee.getName());
  				System.out.println("\t\t"+ee.getText());
  			}
  		}
  	}

  }

  ```

##7.6. XPath语法介绍

###7.6.1. 表达式

- 第一种形式
  - /AAA/DDD/BBB：表示一层一层的，AAA下面DDD下面的BBB
- 第二种形式
  - //BBB：表示和这个名称相同，表示只要名称是BBB，都得到
- 第三种形式
  - /*:所有元素
- 第四种形式
  - BBB[1]：　表示第一个BBB元素
  - BBB[last()]：表示最后一个BBB元素
- 第五种形式
  - //BBB[@id]：表示只要BBB元素上面有id属性，都得到
- 第六种形式
  - //BBB[@id='b1']表示元素名称是BBB,在BBB上面有id属性，并且id的属性值是b1
- 例如：
  - /students/student[@id='1002'] 
  - 根students标记下student标记下属性名为id且属性值为1002的student元素

###7.6.2. dom4j集成xPath操作

- 使用dom4j集成xpath操作

  - 默认的情况下，dom4j不支持xpath，如果想要在dom4j里面是有xpath，引入支持xpath的jar包，如下：

    `jaxen.jar`包

- 在dom4j里面提供了两个方法，用来支持xpath

  - selectNodes("xpath表达式")，获取多个节点
  - selectSingleNode("xpath表达式")，获取一个节点

# 8. 练习

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:element name="HOSPITALS">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element name="HOSPITAL" type="HosType" minOccurs="1" maxOccurs="unbounded" /> </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:complexType name="HosType">
        <xsd:sequence>
            <xsd:element name="NAME" type="xsd:string" />
            <xsd:element name="LOCATION" type="xsd:string" />
            <xsd:element name="PATIENTS" type="PasType" />
            <xsd:element name="DOCTORS" type="DocsType" /> </xsd:sequence>
        <xsd:attributeGroup ref="ID" />
        <xsd:attribute name="KIND" use="required">
            <xsd:simpleType>
                <xsd:restriction base="xsd:string">
                    <xsd:enumeration value="INTERNATIONAL" />
                    <xsd:enumeration value="NATIONAL" />
                    <xsd:enumeration value="PERSONAL" /> </xsd:restriction>
            </xsd:simpleType>
        </xsd:attribute>
    </xsd:complexType>
    <xsd:complexType name="PasType">
        <xsd:sequence>
            <xsd:element name="PATIENT" type="patient" minOccurs="1" maxOccurs="unbounded" /> </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="DocsType">
        <xsd:sequence>
            <xsd:element name="DOCTOR" type="doctor" minOccurs="1" maxOccurs="unbounded" /> </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="patient">
        <xsd:sequence>
            <xsd:element name="ILLNESS" type="xsd:string" />
            <xsd:element name="DOCTOR_ID" type="xsd:string" />
            <xsd:element name="JOINING_DATES" type="xsd:date" />
            <xsd:element name="AGE" type="xsd:int" /> </xsd:sequence>
        <xsd:attributeGroup ref="ID" />
        <xsd:attribute name="BIRTHDAY" use="required" type="xsd:date" /> </xsd:complexType>
    <xsd:complexType name="doctor">
        <xsd:sequence>
            <xsd:element name="MAJOR" type="xsd:string" />
            <xsd:element name="NAME" type="xsd:string" /> </xsd:sequence>
        <xsd:attributeGroup ref="ID" /> </xsd:complexType>
    <xsd:attributeGroup name="ID">
        <xsd:attribute name="ID" type="xsd:ID" /> </xsd:attributeGroup>
</xsd:schema>
```

