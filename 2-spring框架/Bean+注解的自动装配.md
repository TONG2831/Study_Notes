#### Spring Bean
> 1.**Bean在java中的解释**：
    
    1. JavaBean是Java中的一种特殊的类，可以将多个对象封装到一个对象（bean）中。
    
    2. 特点是可序列化，提供无参构造器，提供getter方法和setter方法的访问对象属性。
    
    3. 名字中的Bean是用于Java的可重用软件组件的惯用叫法。
> **2.spring中的Bean的含义**

引用博客上网友的一句话
> Spring里面的bean就类似是定义的一个组件，而这个组件的作用就是实现某个功能的，在spring里给定义的bean就是说，我给你了一个更为简便的方法来调用这个组件去实现你要完成的功能

这是在网上看到的比较通俗易懂的解释.

>**3.对spring控制反转和依赖注入的理解**

Bean的使用和IOC(控制反转)和DI(依赖注入)分不开。
spring的核心功能就包括以上两个，平时的业务逻辑分层有dao、service、action、jdbcTemplate，这几个层次之间层层依赖，例如，在Service中需要创建Dao层的对象，一遍调用方法，实现数据的操作和业务逻辑。

而为了避免这种依赖关系，以达到松耦合的效果，就需要控制反转，将所有的对象创建放在外部容器中创建，然后在本类中提供setXXX（）方法，外部容器通过这种方法实现**注入**。

> **4.实现控制反转**

实现控制反转有三种方法:
1. 普通的set方法，使用自定义BeanFactory,没有使用xml文件
2. xml文件配置+setXX方法
```
<bean id="userDao" class="com.spring.dao.impl.UserDaoImpl" />
	<bean id="userService" class="com.spring.service.impl.UserServiceImpl">
		<property name="userDao" ref="userDao"/>
	</bean>
```
在xml文件中通过property属性，实现注入这一功能

**注意**：
1. property中name的名称，应该和类中setXXX的名称一样，区分大小写
2. 类中的setXXx方法名字应该严格按照getter和setter方法的定义来,set+属性名首字母大写
即按照property的name的值来编写

3.xml+注解@Autowried

```
<!-- 告诉spring 使用注解 -->
	<context:component-scan base-package="com.spring" />

	<bean id="userDao" class="com.spring.dao.impl.UserDaoImpl" />
	<bean id="userService" class="com.spring.service.impl.UserServiceImpl">
	</bean>
```
使用注解的就是为了取代setXX方法或者Bean中的property属性，但是需要在xml文件中添加一行<context:component-scan base-package="com.spring" />

例：在service层中userDao对象上面添加

```
@Resource(name="userDao")
private UserDaoI userDaoI;
```
可以实现自动装配的功能

4.注解

***@Autowried 自动根据类型装配，当配置文件中出现多个同一类型的bean时，按照名称来装配***

说一下@Resource的装配顺序：

- (1)、@Resource后面没有任何内容，默认通过name属性去匹配bean，找不到再按type去匹配
- (2)、指定了name或者type则根据指定的类型去匹配bean
- (3)、指定了name和type则根据指定的name和type去匹配bean，任何一个不匹配都将报错

然后，区分一下@Autowired和@Resource两个注解的区别：
- (1)、@Autowired默认按照byType方式进行bean匹配，@Resource默认按照byName方式进行bean匹配
- (2)、@Autowired是Spring的注解，@Resource是J2EE的注解，这个看一下导入注解的时候这两个注解的包名就一清二楚了
- 

```
@Autowired
    @Qualifier("bmwCar")
    private ICar car;
```

```
@Resource(name="tiger")
private Tiger tiger;
    
@Resource(type=Monkey.class)
private Monkey monkey;
```

6.@service

```
@Service
@Scope("prototype")
public class Zoo {
    
    @Autowired
    private Tiger tiger;
    
    @Autowired
    private Monkey monkey;
    
    public String toString(){
        return tiger + "\n" + monkey;
    }
    
}
```

@Service注解，其实做了两件事情：

(1)、声明Zoo.java是一个bean，这点很重要，因为Zoo.java是一个bean，其他的类才可以使用@Autowired将Zoo作为一个成员变量自动注入。

(2)、Zoo.java在bean中的id是"zoo"，即类名且首字母小写。

(3)、@Scope注解，应该很好理解。因为Spring默认产生的bean是单例的，假如我不想使用单例怎么办，xml文件里面可以在bean里面配置scope属性。注解也是一样，配置@Scope即可，默认是"singleton"即单例，"prototype"表示原型即每次都会new一个新的出来。




