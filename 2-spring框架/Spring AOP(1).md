## Spring AOP

#### 一.基于xml的AOP

> 废话不多说,直接上代码

```xml
<!-- 注册切面 -->
	<bean id="studentLogAspect" class="com.spring.aop.StudentLogAspect" />
	<aop:config>
		指定切面
		<aop:aspect id="logAop" ref="studentLogAspect">
			指定切入点
			<aop:pointcut id="logPointCut" expression="execution(* com.spring.action.StudentAction.*(..))" />
			<aop:before method="myBeforeAdvice" pointcut-ref="logPointCut"/>
			<aop:after method="myAfterReturningAdvice" pointcut-ref="logPointCut"/>
			<aop:after-throwing method="myAfterThrowingAdvice" pointcut-ref="logPointCut" throwing="exception"/>
		</aop:aspect>
	</aop:config> 
```

**注:**

`pointcut` 和`before` 都是放在`aspect` 中

#### 二.@AspectJ

> 使用注解的AOP,定义的(切入点)`pointcut` ,和`advice` (通知),都在是切面中

1. **切入点的定义**

   `pointcut` 使用空的方法定义, 方法的名称即为切入点名称;

   该方法需要满足两点要求:

   - 方法体为空
   - 返回值为void

   ```java
   @Pointcut("execution(* com.spring.action.StudentAction.*(..))")
   private void myPointcut(){}
   ```

2. 以前置通知为例

   ```java
   /**
   	 * 前置通知
   	 */
   	@Before("myPointcut()")
   	public void myBeforeAdvice(JoinPoint joinpoint) {
   		String classAndMethodName = joinpoint.getTarget().getClass().getName() + "类的:"
   				+ joinpoint.getSignature().getName() + "方法";
   		logger.info("前置通知"+classAndMethodName);
   	}
   ```

   

3. **切面Aspect必须在xml注册!!!**

   ```xml
   <bean id="studentLogAspect" class="com.spring.aop.StudentLogAspect" />
   ```


