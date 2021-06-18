# netty_rpc_android

基于netty 封装的client/server 通信组件  aar

## 前言 
NIO 的类库和API繁杂，使用麻烦：需要熟练掌握Selector、ServerSocket、ChannelSocketChannel、 ByteBuffer等。开发工作量和难度都非常大： 例如客户端面临断连重连、 网络闪断、心跳处理、半包读写、 网络拥塞和异常流的处理等等。
Netty 对 JDK 自带的 NIO 的 API 进行了良好的封装，解决了上述问题。且Netty拥有高性能、 吞吐量更高，延迟更低，减少资源消耗，最小化不必要的内存复制等优点

## 使用场景
- 互联网行业：在分布式系统中，各个节点之间需要远程服务调用，高性能的 RPC 框架必不可少，Netty 作为异步高性能的通信框架，往往作为基础通信组件被这些 RPC 框架使用。典型的应用有：阿里分布式服务框架 Dubbo 的 RPC 框架使用 Dubbo 协议进行节点间通信，Dubbo 协议默认使用Netty 作为基础通信组件，用于实现。各进程节点之间的内部通信。Rocketmq底层也是用的Netty作为基础通信组件。
- 游戏行业：无论是手游服务端还是大型的网络游戏，Java 语言得到了越来越广泛的应用。Netty作为高性能的基础通信组件，它本身提供了 TCP/UDP 和 HTTP 协议栈。
- 大数据领域：经典的 Hadoop 的高性能通信和序列化组件 Avro 的 RPC 框架，默认采用 Netty进行跨界点通信，它的 Netty Service 基于 Netty 框架二次封装实现。
- 物联网:手持设备之间近场通信

## 使用方法

- 在项目(最外层)的build.gradle 下面添加仓库地址 

```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://github.com/oceanhub168pub/maven_repo/raw/master" }  // 这一句
    }
}
```
- 在项目中引用 
> api 'com.hub168.yh:netty-rpc-android:1.0.0@aar'

# 客户端

- 当前类实现 IClientStatusable
```
  @Override
    public void onConnectServer(boolean isConnect) {
        Log.e(TAG, "[Client] isConnect: " + isConnect);
        this.isConnect = isConnect;  // 网络能ping通
    }

    @Override
    public void onCreateChannel(String clientId) {
        Log.e(TAG, "[Client] clientId: " + clientId);
        this.clientId = clientId;
    }

    @Override
    public void receiverData(String data) {
        Log.e(TAG, "[Client] receiver:  " + data);
    }
   ```
- 初始化
- 
  NettyClientBootstrap bootstrap = new NettyClientBootstrap(Const.SERVER_PORT, Const.SERVER_URL, IClientStatusable);
  
- 发送消息

  bootstrap.sendData(clientId,文本);
  
  
 # 服务端
 
- 当前类实现 IServerStatusable
```
    @Override
    public void receiverData(String data) {
        Log.e(TAG, "[server][receiverData] receiverData data: " + data);
    }

    @Override
    public void onCreateChannel(String client) {
        Log.e(TAG, "[server][onCreateChannel] client: " + client);
        this.clientId = client;
    }
```
- 初始化

NettyServerBootstrap bootstrap = new NettyServerBootstrap(Const.SERVER_PORT, IServerStatusable);

- 发送数据

 bootstrap.sendData(clientId, 文本内容);





