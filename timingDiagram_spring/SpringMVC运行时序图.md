## SpringMVC运行时序图

### SpringMVC的请求处理流程

​	1.DispathcherServlet是SpringMVC中的前端控制器,负责接收Request并将Request转发给对应的处理组件。

​	Request -> DispatcherServlet - >HandlerMapping  

​	2.HandlerMapping是SpringMVC中完成url到Controller映射的组件，DispathcherServlet接收Request，然后从HandlerMapping查找处理Request的Controller。HandlerMapping的中一个容器是url和controller中method对应的容器。

​	DispatcherServlet ->Controller ->ModelAndView ->DispatcherServlet

​	3.Controller处理Request，并且返回ModelAndView对象（封装结果视图的组件）。

​	DispatcherServlet ->ViewResolver

​	4.将Controller中返回的ViewResolver进行解析

​	DispatcherServlet ->View

​	5.将渲染view返回到浏览器



### SpringMVC九大组件

1.HandlerMappings

​	用来查找Handler的,也就是处理器,具体的表现形式可以是类也可以是方法.标注了@RequestMapping的每一个method都是可以看成是一个handler,handler负责实际的请求处理.HandlerMappings的作用就是当请求到达的之后,它的作用便是找到请求相应的处理器Handler和Interceptors.

2.HandlerAdapters

​	适配器.处理servlet和handler关系.servlet的处理请求的方法是固定的,就是将Handler中各种方法适配servlet中的doService（）方法。

3.HandlerExceptionResolver

​	处理Handler过程产生的异常情况的组件。	

4.ViewResolvers

​	视图解析器；作用是将String类型的视图名和locale解析为View类型的视图.

5.RequestToViewnameTranslator

​	从request中获取viewName,因为ViewResolver是根据ViewName查找View,但有的Handler处理完成之后,没有设置View也没有设置Viewname,就通过这个组件来从Rquest中查找viewName;

6.LocaleResolver

​	从Request中解析出Locale(国际化内容);

7.ThemeResolver

​	解析主题

8.MultipartResolver

​	这个组件是用于处理上传请求 ,将普通的request包装成MultipartHttpServletRerquest来实现,可以通过getFile()直接获得文件,如果是多个文件上传,还可以通过调用getFileMap得到Map<fileName,File>.

9.FlashmapManager

​	FlashMap用于重定向Redirect时参数数据传递.FlashMapManager就是用来管理FlashMap的组件.



### SpringMvc的源码分析

​	1.ApplicationContext初始化时用Map保存所有 url和Controller类的对应关系;

​	2.根据请求url找到对应Controller,并从Controller中找到处理请求的方法

​	3.Request参数绑定到方法的形参,执行方法处理请求,并返回结果视图.



#### 初始化阶段

​	DispatcherServlet中的init()方法,init方法在其父类中HttpServletBean中. 

​	再执行initServletBean()方法 --->onReFresh() -->initStrategies()方法初始换SpringMvc的9大组件.

​	detectHandlers() ->将url和handler进行匹配

#### 调用阶段

​	DispatcherServlet的核心方法doService() -->doDispatch()实现.

​	getHandler()方法实际就是从HandlerMapping中找到url和Controller的对应关系

​	getHandlerAdapter()取处理request的处理器适配器handler adapter

​	applyPreHandle() -->preHandle预拦截

​	handle() - >handleInternal()处理请求 返回ModelAndView;

​	applyPostHandle() ->postHandle()处理modelAndView

​	processDispatchResult()生成页面需要的View

​	render()渲染页面.