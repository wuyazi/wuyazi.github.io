
# Spring

## 概念

## 核心功能

## 其他功能

## 原理

## 借鉴意义



## 单例模式

## 饿汉式/懒汉式

## 事务控制

### 数据库连接要绑定到线程

### 数据库连接在使用时不要关闭，只 commit 和 rollback

## 代理

### 静态代理

### 动态代理

#### JDK 动态代理

#### cglib 动态代理

## Spring IOC

### beans.xml -> BeanFactory

#### 纯 xml （bean 信息定义全部配置在 xml 中）

#### xml + 注解

#### 纯注解模式

### 类图

#### BeanFactory 顶层接口

#### ApplicationContext

#### Spring ioc 实例化 Bean 的三种方式

#### 另外两种方式是为了我们自己 new 的对象加入到 SpringIoC容器

### Bean Scope: singleton(单例), prototype(原型/多例), (request, session, application, websocket: 给 web 用的，但是 web 一般也不用)

#### xml + 注解


1）实际企业开发中，纯xml模式使⽤已经很少了
2）引⼊注解功能，不需要引⼊额外的jar
3）xml+注解结合模式，xml⽂件依然存在，所以，spring IOC容器的启动仍然从加载xml开始
（实际企业使用中很少了）
4）哪些bean的定义写在xml中，哪些bean的定义使⽤注解
#### Lazy-Init 延迟加载

#### FactoryBean 和 BeanFactory

#### 后置处理器

##### BeanPostProcessor

##### BeanFactoryPostProcessor

##### SpringBean 的生命周期图

#### Spring IoC 循环依赖

单例 set 注入方式

解决方式：三级缓存

循环依赖有什么应用场景吗

## AOP

本质：在不改变原有业务逻辑的情况下增强横切逻辑

切面 = 切入点 + 增强
    = 切入点（锁定方法） + 方位点（锁定方法中的特殊实际） + 横切逻辑


编程式事务 - 事务控制代码和业务代码耦合
声明式事务 - 事务控制代码和业务代码分离

事务四大特性 - 完美状态

原子性 （操作角度看：要么都成功要么都失败）
一致性 （数据角度看：转账和要一样）
隔离性 （比如事务还没提交就被读取-脏读）
持久性  (一旦提交)

事务隔离级别 - 解决事务并发问题

脏读
不可重复读 - update 时候
虚读（幻读） - insert | delete

数据库四种隔离界别

串行化 - 早起银行取钱
可重复读 - update 时加行锁
读已提交
读未提交

IoC 和 线程的关系 ??
