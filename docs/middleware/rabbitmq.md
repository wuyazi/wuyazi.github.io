



#### 消息不丢失

##### 消息到达队列

消息的持久化 - 交换机、队列、消息
消费者 ACK 机制
生产者有 confirm 机制（消息是否到达 mq 服务）和return 机制（消息是否路由到指定的 Queue）

失败时
网络问题，重发
业务问题，不重发，直接报 bug 人工预警

#### 补偿机制

DB - （定时任务轮询）
死信队列

客户端如何收到消息
1) 短轮询
2) 长轮询 （nacos， mqtt
3) SSE - Server-Sent Events （单向
4) WebSocket 用的比较多 （双向

