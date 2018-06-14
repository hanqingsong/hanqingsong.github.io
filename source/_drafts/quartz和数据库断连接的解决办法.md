---
title: quartz和数据库断连接的解决办法
date: 2018-05-31 11:57:41
categories:
    - quartz
tags:
    - 后端
    - quartz
---
quartz连接数据库超时会出现以下异常
```
com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: No operations allowed after connection closed.
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
        at com.mysql.jdbc.Util.handleNewInstance(Util.java:404)
        at com.mysql.jdbc.Util.getInstance(Util.java:387)
        at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:917)
        at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:896)
        at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:885)
        at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:860)
        at com.mysql.jdbc.ConnectionImpl.throwConnectionClosedException(ConnectionImpl.java:1246)
        at com.mysql.jdbc.ConnectionImpl.checkClosed(ConnectionImpl.java:1241)
        at com.mysql.jdbc.ConnectionImpl.rollback(ConnectionImpl.java:4564)
        at com.mchange.v2.c3p0.impl.NewProxyConnection.rollback(NewProxyConnection.java:855)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.quartz.impl.jdbcjobstore.AttributeRestoringConnectionInvocationHandler.invoke(AttributeRestoringConnectionInvocationHandler.java:73)
        at com.sun.proxy.$Proxy120.rollback(Unknown Source)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.rollbackConnection(JobStoreSupport.java:3666)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.executeInNonManagedTXLock(JobStoreSupport.java:3825)
        at org.quartz.impl.jdbcjobstore.JobStoreSupport.acquireNextTriggers(JobStoreSupport.java:2756)
        at org.quartz.core.QuartzSchedulerThread.run(QuartzSchedulerThread.java:272)
Caused by: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: The last packet successfully received from the server was 50,340,121 milliseconds ago.  The last packet sent successfully to the server was 50,340,121 milliseconds ago. is longer than the server configured value of 'wait_timeout'. You should consider either expiring and/or testing connection v
alidity before use in your application, increasing the server configured values for client timeouts, or using the Connector/J connection property 'autoReconnect=true' to avoid this problem.
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
        at com.mysql.jdbc.Util.handleNewInstance(Util.java:404)
        at com.mysql.jdbc.SQLError.createCommunicationsException(SQLError.java:988)
        at com.mysql.jdbc.MysqlIO.send(MysqlIO.java:3739)
        at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2508)
        at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2673)
        at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2545)
        at com.mysql.jdbc.ConnectionImpl.setAutoCommit(ConnectionImpl.java:4842)
        at com.mchange.v2.c3p0.impl.NewProxyConnection.setAutoCommit(NewProxyConnection.java:881)
        at org.quartz.impl.jdbcjobstore.AttributeRestoringConnectionInvocationHandler.setAutoCommit(AttributeRestoringConnectionInvocationHandler.java:98)
        at org.quartz.impl.jdbcjobstore.AttributeRestoringConnectionInvocationHandler.invoke(AttributeRestoringConnectionInvocationHandler.java:66)
```
解法方法：
在使用链接时先检查是否有效
```
prop.put("org.quartz.dataSource.quartz.idleConnectionValidationSeconds", 60);
prop.put("org.quartz.dataSource.quartz.validateOnCheckout", true);
prop.put("org.quartz.dataSource.quartz.validationQuery", "select 1");
```

配置说明
```
org.quartz.dataSource.NAME.driver
必须是数据库的JDBC驱动程序的java类名称。

org.quartz.dataSource.NAME.URL
连接到数据库的连接URL（主机，端口等）。

org.quartz.dataSource.NAME.user
连接到数据库时要使用的用户名。

org.quartz.dataSource.NAME.password
连接到数据库时使用的密码。

org.quartz.dataSource.NAME.maxConnections
DataSource可以在其连接池中创建的最大连接数。

org.quartz.dataSource.NAME.validationQuery
是可选的SQL查询字符串，DataSource可用于检测和替换失败/损坏的连接。例如，oracle用户可能会选择“从user_tables中选择table_name” - 这是一个不应该失败的查询 - 除非连接实际上是坏的。

org.quartz.dataSource.NAME.idleConnectionValidationSeconds
空闲连接测试之间的秒数 - 仅在设置验证查询属性时启用。默认值为50秒。

org.quartz.dataSource.NAME.validateOnCheckout
每次从池中检索连接时，是否应该执行数据库sql查询来验证连接，以确保它仍然有效。如果为假，则在办理登机手续时将进行验证。默认值为false。

org.quartz.dataSource.NAME.discardIdleConnectionsSeconds
它们在空闲之后放弃连接几秒钟。0禁用该功能。默认值为0。
```

## 参考资料
https://www.w3cschool.cn/quartz_doc/quartz_doc-d8pn2do9.html
