---
title: shiro使用记录
date: 2018-04-06 18:49:39
categories: 
    - shiro
tags: 
    - shiro
    - 后端
---
## shiro服务器重启之后获取subject getPrincipal为空 getSession不为空问题

这个问题是因为服务器重启session被序列化到本地磁盘，shiro的session实现了接口Serializable，但是自定义的用户实体类（authUser）没有实现接口Serializable所以获取不到Principal，实现接口就好。

## 设置ajax请求不重定向，返回错误码

扩展FormAuthenticationFilter拦截器，重写onAccessDenied

```
 /**
     * 判断所有失败请求
     *
     * @param request
     * @param response
     * @return
     * @throws Exception
     */
    @Override
    protected boolean onAccessDenied(ServletRequest request, ServletResponse response) throws Exception {
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        HttpServletResponse httpServletResponse = (HttpServletResponse) response;
        if (isLoginRequest(request, response)) {
            if (isLoginSubmission(request, response)) {
                if (log.isTraceEnabled()) {
                    log.trace("Login submission detected.  Attempting to execute login.");
                }
                return executeLogin(request, response);
            } else {
                if (log.isTraceEnabled()) {
                    log.trace("Login page view.");
                }
                //allow them to see the login page ;)
                return true;
            }
        } else {
            if (log.isTraceEnabled()) {
                log.trace("Attempting to access a path which requires authentication.  Forwarding to the " +
                        "Authentication url [" + getLoginUrl() + "]");
            }
            PrintWriter out = null;
            try {
                //设置编码
                httpServletResponse.setCharacterEncoding("UTF-8");
                //设置返回类型
                httpServletResponse.setContentType("application/json;charset=UTF-8");
                out = httpServletResponse.getWriter();
                out.println("{\"statusCode\": 401,\"statusText\": \"认证失败\"}");
            } catch (Exception e) {
                log.error("{}", e.getMessage(), e);
            } finally {
                if (null != out) {
                    out.flush();
                    out.close();
                }
            }
            return false;
        }
    }
```

## 开启shiro支持注解

```
/**
     * 开启shiro 注解支持
     *
     * @param securityManager
     * @return
     */
    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        System.out.println("开启了Shiro注解支持");
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }
```