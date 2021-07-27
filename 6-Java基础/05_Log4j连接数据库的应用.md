#### demo需求：
    实际开发中，可查的日志文件能有效地帮助开发人员进行问题排查，而数据库相对于文件更显得有通用性，所以我们这里做一个Log4j将日志插入数据库的demo。
    
#### demo主要技术：
    Log4j的使用，如有不会，可以查看Java技术Log4j部分笔记，更深入的请百度。
    
#### demo所用jar包：
    log4j-1.2.17.jar
    mysql-connector-java-5.1.7-bin.jar

#### demo主要代码：
``` Java
public class Test {

	public static void main(String[] args) {
	    Logger logger = Logger.getLogger(Test.class);
		    logger.error("hello");
	}
}
``` 
    
``` properties
log4j.rootLogger=error,Console,DB

### Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss:SSS}[%p]: %m%n

### DB
log4j.appender.DB=org.apache.log4j.jdbc.JDBCAppender
log4j.appender.DB.URL=jdbc:mysql://localhost/logs
log4j.appender.DB.driver=com.mysql.jdbc.Driver
log4j.appender.DB.user=root
log4j.appender.DB.password=123456
log4j.appender.DB.sql=INSERT INTO LOGS VALUES('%x','%d{yyyy-MM-dd HH:mm:ss}','%C','%p','%m')
log4j.appender.DB.layout=org.apache.log4j.PatternLayout 
```

#### demo资源位置：
    svn://106.15.229.200/Javaweb（项目名：tinyDemo_log4j，用户 xiaoyezi/xiaoyezi）