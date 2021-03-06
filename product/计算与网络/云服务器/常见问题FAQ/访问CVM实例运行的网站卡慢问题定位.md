## 访问CVM实例运行的网站卡慢问题定位
一次完整的http请求包括域名解析、建立TCP连接、发起请求、服务器接收到请求进行处理并返回处理结果、浏览器对html代码进行解析并请求其他资源、最后对页面进行渲染呈现。这其中经历了用户本地客户端、客户端到接入服务器之间的网络节点以及服务器，这三个环节中的任意一个出现问题，都有可能导致网站访问卡慢。
### 1.本地客户端问题确认
本地客户端访问播测网站(ping.huatuo.qq.com)，测试本地访问各域名的速度，确认本地网络是否存在问题。测试结果如下图，从结果中可以获知访问各个域名的延迟，以及网络是否正常。如果不正常请联系您的网络服务提供上进行协助定位解决。
![](https://mc.qcloudimg.com/static/img/147fa13722d12163f4fc0ca6ad40df81/image.png)
### 2.网络链路问题确认
如果第一步确认没有异常，进一步确认本地客户端到服务器之前网络是否有问题。
1）本地客户端ping服务器公网IP，确认是否存在丢包或延时高的情况
2）如果存在丢包或时延高的情况，进一步使用MTR进行诊断。具体参考《服务器网络延迟和丢包定位》。
3）如果ping服务器IP无异常，可以使用dig/nslookup看下DNS的解析情况，排查是否DNS解析引起的问题。也可以通过直接使用IP访问对应页面，排查是否DNS的问题导致访问慢。
### 3.服务器问题确认
如果客户端和网络链路都没有问题，进一步对Web服务器进行分析。是否系统资源不足、中病毒木马或者被DDOS攻击了。
1）在实例的详情，点击监控tab，可以查看实例资源使用情况。
![](https://mc.qcloudimg.com/static/img/fd32ca7361dc89f56ee8d51ff72dca4d/image.png)
2）如果CPU/内存/带宽/磁盘使用率过高，可能是服务器自身负载较高或者中毒等问题导致，请参考对应的文档进行排查。
《CPU使用率过高排查(Linux系统)》
《CPU使用率过高排查(Windows系统)》
《带宽利用率过高问题定位》
### 4.业务问题确认
1）如果3定位到是服务器负载引起的资源消耗增大，则属于正常情况。可以通过优化业务程序，抑或升级现有的服务器配置或购买新的服务器分担现有服务器的压力解决。
2）如果上述3步都正常，则建议查看日志文件，定位具体是哪一步导致服务器响应慢，进行针对性的优化。

