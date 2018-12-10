### spring mvc执行流程
![aa](../../imgs/20181206-1.png)

```angular2html
1.用户发送请求至前端控制器DispatcherServlet
2.DispatcherServlet收到请求调用处理器映射器HandlerMapping。
3.处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。
4.DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
5.执行处理器Handler(Controller，也叫页面控制器)。
6.Handler执行完成返回ModelAndView
7.HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
8.DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9.ViewReslover解析后返回具体View
10.DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。
11.DispatcherServlet响应用户。
```
    