
![[Pasted image 20240415220047.png]]
Http请求到 DispatchServlet 中，流程如下：
1. 通过 `HandlerMapping` 拿到 request 对应的 `HandlerExecutionChain`，然后再拿到 `HandlerExecutionChain` 中 handler 对应的 `HandlerAdapter`，执行 `HandlerExecutionChain` 中 `interceptor#preHandle` 方法 （责任链模式）
2. 通过 `HandlerAdapter` 去执行 handler，handler 对应的是之前注册的 `HandlerMethod`（HandlerMethod里面封装的映射的真正方法 handler，还有可能是原生的 Servlet），所以要执行 `handler.invoke`，在这之前要判断参数，需要参数解析器 `HandlerMethodArgumentResolver`。反射调用完成后，需要调用返回值解析器 `HandlerMethodReturnValueHandler`（适配器&组合模式&策略模式）
3. 真正方法执行完成后，再执行 `HandlerExecutionChain` 中 `interceptor#postHandle` 方法进行拦截器的后置处理
4. Spring MVC执行完成后返回的是 `ModelAndView`，还需要对 `ModelAndView` 进行 渲染，即把 `ModelAndView` 中的 View 渲染到 response 中
5. 当发生异常事，会将异常拉倒用户业务自己的异常处理方法中，这时也需要对参数和返回值进行 custom，此时就需要用到 HandlerExceptionResolver 系列了。因为用户标记的 @ExceptionHandler 方法已经被 ExceptionHandlerMethodResolver 找到并注册（key为对应异常，value为对应方法）只需要调用该方法就可以对异常进行处理，此时的方法调用和之前的 Handler 几乎没有区别