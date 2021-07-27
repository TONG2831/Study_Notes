# ApplicationContextAware

## 1.概述

> 在Spring中有很多继承于`Aware`的接口，`ApplicationContextAware`就是其中一个。
>
> `Aware`译：“已感知的”、“意识到的”。那么`ApplicationContextAware`可以认识是可以感知	`ApplicationContext`的接口。
>
> **问题：**`Aware`和Spring的`IOC`,`DI`有什么关联，或者如何去从这两个方面理解`Aware`？

## 2.如何使用

开发人员通过继承`ApplicationContextAware`接口,实现接口中的`setApplicationContext`方法，在Bean初始化的的时候，会将`ApplicationContext`的实例,通过该方法给当前实现类中ApplicationContextAware的引用。

```java
public class ApplicationContextUtils implements ApplicationContextAware {

    private static ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        ApplicationContextUtils.context = applicationContext;
    }

    public static ApplicationContext getContext(){
        return ApplicationContextUtils.context;
    }
}
```

