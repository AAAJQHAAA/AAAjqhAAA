---
title: 日志
date: 2023-11-10 13:48:51
category: 后端
tags:
  - springboot日志
  - 系统日志
  - 操作日志
---
# 日志
- https://zhuanlan.zhihu.com/p/641027899
- https://blog.csdn.net/xiaoheihai666/article/details/130772577
- https://blog.csdn.net/qq_42102911/article/details/130647814
## 系统日志
- 程序执行时输出的debug、info、warn、error等不同级别的程序执行记录信息；
- 给程序员或运维看的，一般在出现异常问题的时候，可以通过系统日志中记录的关键参数信息和异常提示，快速排除故障。
## 操作日志
- 用户实际业务操作行为的记录，这些信息一般存储在数据库里，如什么时间哪个用户点了某个菜单、修改了哪个配置等这类业务操作行为，这些日志信息是给普通用户或系统管理员看到。
- 需求分析
  - 1、记录用户的业务操作行为，记录的字段有：操作人、操作时间、操作功能、日志类型、操作内容描述、操作内容报文、操作前内容报文
  - 2、提供一个可视化的页面，可以查询用户的业务操作行为，对重要操作回溯；
  - 3、提供一定的管理功能，必要的时候可以对用户的误操作回滚；
- 反面实现
  - 1、每个接口里都加一段记录业务操作日志的记录；
  - 2、每个接口里都要捕获一下异常，记录异常业务操作日志；
  - 硬编码实现的业务操作日志管理功能的问题
    - 业务操作日志收集与业务逻辑耦合严重
    - 代码重复，新开发的接口在完成业务逻辑后要织入一段业务操作日志保存的逻辑
    - 已开发上线的接口，还要重新再修改织入业务操作日志保存的逻辑并测试，且每个接口需要织入的业务操作日志保存的逻辑是一样的
- 正面实现
  - 方案一：javax.servlet.Filter（过滤器）
    - 依赖于servlet容器
    - 基于函数回调实现
    - 拦截URL对应的请求request和响应response
    - 结论：【不适用细节记录】
  - 方案二：org.springframework.web.servlet.HandlerInterceptor（拦截器）
    - 依赖于SpringMVC框架
    - 基于Java的反射机制实现（AOP思想）
    - 
  - 方案三：Spring AOP（切面）

```java
package javax.servlet;

import java.io.IOException;

public interface Filter {

    /**
     * 过滤器实例初始化（只调用一次）
     * 
     * @param filterConfig 与正在初始化的过滤器实例相关联的配置信息
     *
     * @throws ServletException 初始化失败抛异常
     */
    default void init(FilterConfig filterConfig) throws ServletException {}

    /**
     * 每次请求时，容器都会调用doFilter方法
     * 此方法的典型实现将遵循以下模式:
     * 1。检查请求
     * 2。修改请求对象，请求头
     * 3。修改响应对象，响应头
     * 4-1。使用FilterChain对象chain. dofilter()调用链中的下一个实体，
     * 4-2。或者不将请求、响应对传递给过滤器链中的下一个实体以阻止请求处理
     *
     * @param request  要处理的请求
     * @param response 与请求相关联的响应
     * @param chain    链式调用，下一过滤器的访问
     *
     * @throws IOException      如果在此过滤器处理请求期间发生I/O错误
     * @throws ServletException 如果处理因任何其他原因失败
     */
    void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;

    /**
     * 由web容器调用，表示过滤器正在退出。
     * 此方法可以来清理所有被占用的资源(例如，内存、文件句柄、线程)，并确保任何持久状态都与过滤器在内存中的当前状态同步。
     */
    default void destroy() {}
}
```

```java
package org.springframework.web.servlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.lang.Nullable;
import org.springframework.web.method.HandlerMethod;

/**
 * Workflow interface that allows for customized handler execution chains.
 * Applications can register any number of existing or custom interceptors for certain groups of handlers, to add common preprocessing behavior without needing to modify each handler implementation.
 *
 * A HandlerInterceptor gets called before the appropriate HandlerAdapter triggers the execution of the handler itself. 
 * This mechanism can be used for a large field of preprocessing aspects, e.g. for authorization checks, or common handler behavior like locale or theme changes. 
 * Its main purpose is to allow for factoring out repetitive handler code.
 *
 * In an asynchronous processing scenario, the handler may be executed in a separate thread while the main thread exits without rendering or invoking the {@code postHandle} and {@code afterCompletion} callbacks. 
 * When concurrent handler execution completes, the request is dispatched back in order to proceed with rendering the model and all methods of this contract are invoked again. 
 * For further options and details see {@code org.springframework.web.servlet.AsyncHandlerInterceptor}
 *
 * Typically an interceptor chain is defined per HandlerMapping bean,sharing its granularity. To be able to apply a certain interceptor chain to a group of handlers, one needs to map the desired handlers via one HandlerMapping bean. The interceptors themselves are defined as beans in the application context, referenced by the mapping bean definition via its "interceptors" property.
 *
 * HandlerInterceptor is basically similar to a Servlet Filter, but in contrast to the latter it just allows custom pre-processing with the option of prohibiting the execution of the handler itself, and custom post-processing.
 * Filters are more powerful, for example they allow for exchanging the request and response objects that are handed down the chain. Note that a filter gets configured in web.xml, a HandlerInterceptor in the application context.
 *
 * As a basic guideline, fine-grained handler-related preprocessing tasks are candidates for HandlerInterceptor implementations, especially factored-out common handler code and authorization checks. 
 * On the other hand, a Filter is well-suited for request content and view content handling, like multipart forms and GZIP compression. This typically shows when one needs to map the filter to certain content types (e.g. images), or to all requests.
 *
 */
public interface HandlerInterceptor {

	/**
	 * Intercept the execution of a handler. Called after HandlerMapping determined an appropriate handler object, but before HandlerAdapter invokes the handler.
	 * DispatcherServlet processes a handler in an execution chain, consisting of any number of interceptors, with the handler itself at the end.
	 * With this method, each interceptor can decide to abort the execution chain, typically sending an HTTP error or writing a custom response.
	 * Note: special considerations apply for asynchronous request processing. For more details see {@link org.springframework.web.servlet.AsyncHandlerInterceptor}.
	 * The default implementation returns {@code true}.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @param handler chosen handler to execute, for type and/or instance evaluation
	 * @return {@code true} if the execution chain should proceed with the next interceptor or the handler itself. Else, DispatcherServlet assumes that this interceptor has already dealt with the response itself.
	 * @throws Exception in case of errors
	 */
	default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		return true;
	}

	/**
	 * Intercept the execution of a handler. Called after HandlerAdapter actually invoked the handler, but before the DispatcherServlet renders the view.
	 * Can expose additional model objects to the view via the given ModelAndView.
	 * DispatcherServlet processes a handler in an execution chain, consisting of any number of interceptors, with the handler itself at the end.
	 * With this method, each interceptor can post-process an execution, getting applied in inverse order of the execution chain.
	 * Note: special considerations apply for asynchronous request processing. For more details see {@link org.springframework.web.servlet.AsyncHandlerInterceptor}.
	 * The default implementation is empty.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @param handler the handler (or {@link HandlerMethod}) that started asynchronous execution, for type and/or instance examination
	 * @param modelAndView the {@code ModelAndView} that the handler returned (can also be {@code null})
	 * @throws Exception in case of errors
	 */
	default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {}

	/**
	 * Callback after completion of request processing, that is, after rendering the view. Will be called on any outcome of handler execution, thus allows for proper resource cleanup.
	 * Note: Will only be called if this interceptor's {@code preHandle} method has successfully completed and returned {@code true}!
	 * As with the {@code postHandle} method, the method will be invoked on each interceptor in the chain in reverse order, so the first interceptor will be the last to be invoked.
	 * Note: special considerations apply for asynchronous request processing. For more details see {@link org.springframework.web.servlet.AsyncHandlerInterceptor}.
	 * The default implementation is empty.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @param handler the handler (or {@link HandlerMethod}) that started asynchronous execution, for type and/or instance examination
	 * @param ex any exception thrown on handler execution, if any; this does not include exceptions that have been handled through an exception resolver
	 * @throws Exception in case of errors
	 */
	default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {}
}
```