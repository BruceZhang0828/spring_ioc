## 一步一步手绘Spring Mvc运行时序图

###  内容定位

​	1.先掌握执行流程,再掌握设计思想

​	2.要多看几遍

### spring mvc处理流程

​	1.Request请求 -> servlet接受

​	2.HandlerMapping匹配url到指定的Controller

​	3.Controller 进行业务处理返回的ModelAndView

​	4.ModelAndView --> ViewResolver处理

​	5.View 经过Response返回到浏览器

### Spring Mvc的九大组件

​	1.HandlerMappings :具体处理方法和url的匹配管理

​	2.HanderAdapters:request中的参数与方法参数的转换

​	3.HandlerExceptionResolvers:异常处理

​	4.ViewResolvers:视图解析器,View resolveViewName():View对象,View的render()在渲染为Html页面的语法。

​	5.RequestToViewNameTranslator：将请求解析为一个展示的结果

​	6.LocaleResolver：从Request解析的Locale（语言解析器）与国际化有关系.

​	7.ThemeResolver:解析request中的主题

​	8.MultipartResolver:文件上传的解析,返回的一个MultipatResolverRquest的对象里边存在一个getFile的方法;Map<String,MutipartFile>

​	9.FlashMapManager:FalshMap的管理类;Flash => 闪存;redirect时做为一个参数传递。就是参数中转将参数放入到request的attribute中。

### SpringMvc源码分析

初始化阶段：

​	DisPatchServlet开始 --》HttpServletBean——》init（）-》FrameworkServlet -> initServletBean() ->initWebApplicationContext() --->DIspatcherServlet ->onrefresh() -> initStrateqies()//初始化SpringMvc的9大组件;

initHandlerMapping() ->List<HandlerMapping>

调用阶段：

​	DisPatchServlet -> doService() ->doDispatch() -> getHander()//获取handlerMapping --> AbstractHandlerMapping  ->getHandlerExcutionChain() -->DispatchServlet -> getHandlerAdpater()  -->Handlerinterceptor ->preHandle ()//预拦截 --> RequestMappingHandlerAdapter() ->handle()->getModleAndView() -->DispatchServlet   