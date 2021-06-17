
## Tomcat 介绍

### 系统架构

浏览器访问服务器使⽤的是Http协议，Http是应⽤层协议，⽤于定义数据通信的格式，具体的数
据传输使⽤的是TCP/IP协议

#### 架构设计

![架构设计](020101.png)

Tomcat的两个重要身份
1）http服务器
2）Tomcat是⼀个Servlet容器（实现了 Servlet 接口）

#### 主要组件

![架构设计](020102.png)

Coyote 是Tomcat 中连接器的组件名称 , 是对外的接⼝。客户端通过Coyote与服务器建⽴连接、发送请
求并接受响应 。
（1）Coyote 封装了底层的⽹络通信（Socket 请求及响应处理）
（2）Coyote 使Catalina 容器（容器组件）与具体的请求协议及IO操作⽅式完全解耦
（3）Coyote 将Socket 输⼊转换封装为 Request 对象，进⼀步封装后交由Catalina 容器进⾏处理，处
理请求完成后, Catalina 通过Coyote 提供的Response 对象将结果写⼊输出流
（4）Coyote 负责的是具体协议（应⽤层）和IO（传输层）相关内容

![架构设计](020103.png)

#### Coyote 组件及作⽤

![架构设计](020104.png)

#### Catalina 组件及作⽤

![架构设计](020105.png)

### Tomcat 配置

## Tomcat 原理



