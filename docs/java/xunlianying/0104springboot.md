 SpringBoot Starter：他将常用的依赖分组进行了整合，将其合并到一个依赖中，这样就可以一次
性添加到项目的Maven或Gradle构建中

热部署 restart vc reload (base-classloader  restart-classloader)


选择IDEA工具界面的【File】->【Settings】选项，打开Compiler面板设置页面，选中 Build project automaticaly
中使用组合快捷键“Ctrl+Shift+Alt+/”打开Maintenance选项框，选中“compiler.automake.allow.when.app.running”


Spring Boot使用一个application.properties或者application.yaml的文件作为全局配置文件
–file:./config/
–file:./
–classpath:/config/
–classpath:/




给第三方类使用配置的值
先实例化对象，@Bean，再设置前缀


日志框架

记录运行时的关键信息，不用每个函数添加
异步模式
自动归档

参考 （JDBC-数据库驱动 模型），抽象层和实现层分离

Spring 框架选择使用了 JCL 作为默认日志输出。而 Spring Boot 默认选择了 SLF4J 结合 LogBack

统一日志框架

遗留问题：A项目（slf4J + logback）: Spring（commons logging）、Hibernate（jboss-logging）、
mybatis....

统一日志框架使用步骤归纳如下：
1. 排除系统中的其他日志框架。
2. 使用中间包替换要替换的日志框架。
3. 导入我们选择的 SLF4J 实现。

查看依赖

默认日志级别： Info
日志组成

自定义日志输出

```
# 日志配置
# 指定具体包的日志级别
logging.level.com.lagou=debug
# 控制台和日志文件输出格式
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level
%logger{50} - %msg%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50}
- %msg%n
# 日志输出路径，默认文件spring.log
logging.file.path=spring.log
#logging.file.name=log.log

```

替换日志框架（实现层）

### 源码剖析

依赖管理
自动配置
@SpringBootApplication -> @SpringBootConfiguration -> @Configuration
@EnableAutoConfiguration -> @Import -> AutoConfigurationImportSelector -> META-INF/spring.factories
@EnableAutoConfiguration -> @AutoConfigurationPackage
@ConditionalOnClass , @ConditionalOnBean 或 @ConditionalOnWebApplication


最后，我们再总结下SpringBoot自动配置的原理，主要做了以下事情：
1. 从spring.factories配置文件中加载自动配置类；
2. 加载的自动配置类中排除掉 @EnableAutoConfiguration 注解的 exclude 属性指定的自动配置
类；
3. 然后再用 AutoConfigurationImportFilter 接口去过滤自动配置类是否符合其标注注解（若有
标注的话） @ConditionalOnClass , @ConditionalOnBean 和
@ConditionalOnWebApplication 的条件，若都符合的话则返回匹配结果；
4. 然后触发 AutoConfigurationImportEvent 事件，告诉 ConditionEvaluationReport 条件评
估报告器对象来分别记录符合条件和 exclude 的自动配置类。
5. 最后spring再将最后筛选后的自动配置类导入IOC容器中


run 方法六大主要步骤
一、获取并启动监听器 - java listener 启动原理 ？？
二、构造应用上下文环境
三、初始化应用上下文 - 创建 context 和 IoC
四、刷新应用上下文前的准备阶段
五、刷新应用上下文 - invokeBeanFactoryPostProcessors(beanFactory), SPI 扩展机制, this.group.selectImports 怎么调到 spring-boot 的
六、刷新应用上下文后的扩展接口 - 重写这个方法


#### SpringBoot starter机制
SpringBoot中的starter是一种非常重要的机制，能够抛弃以前繁杂的配置，将其统一集成进
starter，应用者只需要在maven中引入starter依赖，SpringBoot就能自动扫描到要加载的信息并
启动相应的默认配置。starter让我们摆脱了各种依赖库的处理，需要配置各种信息的困扰。
SpringBoot会自动通过classpath路径下的类发现需要的Bean，并注册进IOC容器。SpringBoot提
供了针对日常企业应用研发各种场景的spring-boot-starter依赖模块。所有这些依赖模块都遵循着
约定成俗的默认配置，并允许我们调整这些配置，即遵循“约定大于配置”的理念。



集成 MyBatis
sqlSessionFactory
sqlSessionTemplate

mapperScan

动态数据源

### 缓存

JSR 107
Java Caching（JSR-107）定义了5个核心接口，分别是CachingProvider、CacheManager、
Cache、Entry和Expiry。
CachingProvider（缓存提供者）：创建、配置、获取、管理和控制多个CacheManager
CacheManager（缓存管理器）：创建、配置、获取、管理和控制多个唯一命名的Cache，
Cache存在于CacheManager的上下文中。一个CacheManager仅对应一个CachingProvider
Cache（缓存）：是由CacheManager管理的，CacheManager管理Cache的生命周期，
Cache存在于CacheManager的上下文中，是一个类似map的数据结构，并临时存储以key为
索引的值。一个Cache仅被一个CacheManager所拥有
Entry（缓存键值对）：是一个存储在Cache中的key-value对
Expiry（缓存时效）：每一个存储在Cache中的条目都有一个定义的有效期。一旦超过这个
时间，条目就自动过期，过期后，条目将不可以访问、更新和删除操作。缓存有效期可以通
过ExpiryPolicy设置

cacheManager

如何在事务里更新缓存???

@Cacheable
key 可以用 spEL 计算 key 的值
cacheNames/value
指定缓存的名字，缓存使用CacheManager管理多个缓存组件
Cache，这些Cache组件就是根据这个名字进行区分的。对缓存的真
正CRUD操作在Cache中定义，每个缓存组件Cache都有自己唯一的
名字，通过cacheNames或者value属性指定，相当于是将缓存的键
值对进行分组，缓存的名字是一个数组，也就是说可以将一个缓存
键值对分到多个组里面
key
缓存数据时的key的值，默认是使用方法参数的值，可以使用SpEL表
达式计算key的值
keyGenerator
缓存的生成策略，和key二选一，都是生成键的，keyGenerator可自
定义
cacheManager 指定缓存管理器(如ConcurrentHashMap、Redis等)
cacheResolver 和cacheManager功能一样，和cacheManager二选一
condition 指定缓存的条件(满足什么条件时才缓存)，可用SpEL表达式(如id>0，表示当入参id大于0时才缓存)
unless
否定缓存，即满足unless指定的条件时，方法的结果不进行缓存，
使用unless时可以在调用的方法获取到结果之后再进行判断(如result==null，表示如果结果为null时不缓存)
sync 是否使用异步模式进行缓存

@CachePut

@CacheEvict (删除操作，先请缓存再删数据？)

@CacheConfig

AutoConfiguration vs Option


### 部署

#### 部署打包
推荐打成 Jar 包部署，（内置 Tomcat，环境统一）

打jar包时不会把src/main/webapp 下的内容打到jar包里 (你认为的打到jar包里面，路径是不行的会报404)
打war包时会把src/main/webapp 下的内容打到war包里

打成什么文件包进行部署与项目业务有关，就像提供 rest 服务的项目需要打包成 jar文件，用命令运行很方便。。。
而有大量css、js、html，且需要经常改动的项目，打成 war 包去运行比较方便，
因为改动静态资源可以直接覆盖，很快看到改动后的效果，这是 jar 包不能比的

#### 多环境部署

@Profile("prod")
多资源配置文件


Actuator 是一个监控工具

原生端点和用户自定义端点
应用配置类
度量指标类
操作控制类（要做安全管理）

management.endpoints.web.exposure.include=*

info.app.name=spring-boot-actuator

Spring Boot Admin

