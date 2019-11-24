# Context

Trying to use self-signed certificate, that exposes a /hello endpoint returning "Hello World".

## Issue
When I cURL the */hello* endpoint I get the right response :
```sh 
curl --cacert test.crt.pem https://localhost:8441/hello
Hello world%
```

But when I call the */hello* endpoint from the browser I got 

```java
io.netty.handler.codec.DecoderException: javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_unknown
	at io.netty.handler.codec.ByteToMessageDecoder.callDecode(ByteToMessageDecoder.java:473) ~[netty-codec-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.handler.codec.ByteToMessageDecoder.channelRead(ByteToMessageDecoder.java:281) ~[netty-codec-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:374) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:360) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.AbstractChannelHandlerContext.fireChannelRead(AbstractChannelHandlerContext.java:352) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.DefaultChannelPipeline$HeadContext.channelRead(DefaultChannelPipeline.java:1422) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:374) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.AbstractChannelHandlerContext.invokeChannelRead(AbstractChannelHandlerContext.java:360) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.DefaultChannelPipeline.fireChannelRead(DefaultChannelPipeline.java:931) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.nio.AbstractNioByteChannel$NioByteUnsafe.read(AbstractNioByteChannel.java:163) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.nio.NioEventLoop.processSelectedKey(NioEventLoop.java:700) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.nio.NioEventLoop.processSelectedKeysOptimized(NioEventLoop.java:635) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.nio.NioEventLoop.processSelectedKeys(NioEventLoop.java:552) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:514) ~[netty-transport-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.util.concurrent.SingleThreadEventExecutor$6.run(SingleThreadEventExecutor.java:1050) ~[netty-common-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74) ~[netty-common-4.1.43.Final.jar:4.1.43.Final]
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30) ~[netty-common-4.1.43.Final.jar:4.1.43.Final]
	at java.base/java.lang.Thread.run(Thread.java:834) ~[na:na]
```

### Project description

Port number : 8441
Java version : 11
Spring boot version : 2.2.1
Dependencies : 
 - lombok
 - webflux

### Keystore Info : 

Keystore is generated like following :

```sh
$ keytool -genkeypair -alias test -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore test.p12 -validity 3650 ``
Enter keystore password:  
Re-enter new password: 
What is your first and last name?
  [Unknown]:  localhost
What is the name of your organizational unit?
  [Unknown]:  localhost
What is the name of your organization?
  [Unknown]:  localhost
What is the name of your City or Locality?
  [Unknown]:  Paris
What is the name of your State or Province?
  [Unknown]:  Paris
What is the two-letter country code for this unit?
  [Unknown]:  fr
Is CN=localhost, OU=localhost, O=localhost, L=Paris, ST=Paris, C=fr correct?
  [no]:  yes
```

cert are generated using the command : 

```sh 
openssl pkcs12 -in test.p12 -out test.crt.pem -clcerts -nokeys
```

### Browser info 

Google Chrome	78.0.3904.108 (Official Build) (64-bit)
OS	macOS Version 10.15.1 (Build 19B88)