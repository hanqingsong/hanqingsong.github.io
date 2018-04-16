title: shiro简介
categories: 后端
tags:
  - shiro
  - java
date: 2018-03-27 17:07:13
---

## 什么是Apache Shiro？

Apache Shiro 是一个强大易用的Java安全框架，提供了认证、授权、加密和会话管理功能，可为任何应用提供安全保障 - 从命令行应用、移动应用到大型网络及企业应用。

## Shiro能做的事情

- 登录验证
- 访问控制，如： 
    - 判断用户是否拥有角色admin
    - 判断用户是否拥有访问的权限
- 在任何环境下使用 Session API
- 可以使用多个用户数据源。例如一个是oracle用户库，另外一个是mysql用户库
- 单点登录（SSO）功能
- “Remember Me”服务 ，类似购物车的功能，shiro官方建议开启
- 可以直接使用annotation对所使用的方法做权限控制
- 可以在jsp中通过shiro标签方便的做到细粒度的权限控制

## 四大API

- Authentication —— 认证，用户身份识别，常被称为用户“登录”，who are you?
- Authorization —— 授权，访问控制过程，决定“谁”访问“什么”，who can do what?
- Session Management —— 会话管理，用户session管理器，用户相关的时间敏感的状态
- Cryptography —— 密码加密，把JDK中复杂的密码加密方式进行封装，保护或隐藏数据防止被偷窥
