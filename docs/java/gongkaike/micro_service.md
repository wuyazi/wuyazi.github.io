

1. 响应时间(RT) 
　　响应时间是指系统对请求作出响应的时间。对于单用户的系统，响应时间可以很好地度量系统的性能。

2. 吞吐量(Throughput) 
     吞吐量是指系统在单位时间内处理请求的数量。对于并发系统，通常需要用吞吐量作为性能指标。 

3. 并发用户数 
　　并发用户数是指系统可以同时承载的正常使用系统功能的用户的数量。与吞吐量相比，并发用户数是一个更直观但也更笼统的性能指标。实际上，并发用户数是一个非常不准确的指标，因为用户不同的使用模式会导致不同用户在单位时间发出不同数量的请求。
4. QPS每秒查询率(Query Per Second) 
　　每秒查询率QPS是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准，在因特网上，作为域名系统服务器的机器的性能经常用每秒查询率来衡量。对应fetches/sec，即每秒的响应请求数，也即是最大吞吐能力。 （看来是类似于TPS，只是应用于特定场景的吞吐量）



分布式 -> 微服务
绞杀模式


