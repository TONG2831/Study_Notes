## SpringMVC源码阅读

### 一、重要对象解读

#### 1.HandlerMapping -处理器映射器

handler：使用 `@Controller` 和 `@RequestMapping`注解的类或方法

```
接口，interface
其实现类，定义了请求地址（request）与处理器（handler）之间的映射关系

```
#### 2.HandlerInterceptor -处理器拦截器

定义
```
1.接口
2.在自定义处理器执行链中被允许的工作流接口
3.应用程序中，可以为一组处理器注册任意数量的自有的或自定义的拦截器。
4.可以添加常见的预处理行为，无需修改每个处理器的实现
```

