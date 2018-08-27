



# Telnet (new version) Command Usage 

dubbo 2.5.8 version reconstructs the telnet module, providing new telnet command support. 

### Port
the port of new version telnet is different from the port of dubbo agreement. The default port is `22222`, which can be changed by modifying configuration`dubbo.properties` 

```
dubbo.application.qos.port=33333
```

or by modifying the JVM parameter

```
-Ddubbo.application.qos.port=33333
```

### Safety

In default situation, dubbo can receive any command sent from the host computer, which can be changed by modifying the configuration `dubbo.properties`  

```
dubbo.application.qos.accept.foreign.ip=false
```

or by modifying the JVM parameter

```
-Ddubbo.application.qos.accept.foreign.ip=false
```

to reject command sent from the remote host computer, allowing only the local server to act on the command 


### Telnet agreement and http agreement 

The telnet module supports both http agreement and telnet agreement for usgae convenience under most situations. 

For example, 

```
➜  ~ telnet localhost 22222
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
  ████████▄  ███    █▄  ▀█████████▄  ▀█████████▄   ▄██████▄
  ███   ▀███ ███    ███   ███    ███   ███    ███ ███    ███
  ███    ███ ███    ███   ███    ███   ███    ███ ███    ███
  ███    ███ ███    ███  ▄███▄▄▄██▀   ▄███▄▄▄██▀  ███    ███
  ███    ███ ███    ███ ▀▀███▀▀▀██▄  ▀▀███▀▀▀██▄  ███    ███
  ███    ███ ███    ███   ███    ██▄   ███    ██▄ ███    ███
  ███   ▄███ ███    ███   ███    ███   ███    ███ ███    ███
  ████████▀  ████████▀  ▄█████████▀  ▄█████████▀   ▀██████▀


dubbo>ls
As Provider side:
+----------------------------------+---+
|       Provider Service Name      |PUB|
+----------------------------------+---+
|com.alibaba.dubbo.demo.DemoService| N |
+----------------------------------+---+
As Consumer side:
+---------------------+---+
|Consumer Service Name|NUM|
+---------------------+---+

dubbo>
```


```
➜  ~ curl "localhost:22222/ls?arg1=xxx&arg2=xxxx"
As Provider side:
+----------------------------------+---+
|       Provider Service Name      |PUB|
+----------------------------------+---+
|com.alibaba.dubbo.demo.DemoService| N |
+----------------------------------+---+
As Consumer side:
+---------------------+---+
|Consumer Service Name|NUM|
+---------------------+---+
```

### ls List consumers and providers by Is
改：Is列出消费者和生产者

```
dubbo>ls
As Provider side:
+----------------------------------+---+
|       Provider Service Name      |PUB|
+----------------------------------+---+
|com.alibaba.dubbo.demo.DemoService| Y |
+----------------------------------+---+
As Consumer side:
+---------------------+---+
|Consumer Service Name|NUM|
+---------------------+---+
```

改：列出dubbo为生产者和消费者所提供的服务
List the services that dubbo provides to providers and consumers

### Online service command 

改：当使用延迟发布功能的时候(通过设置com.alibaba.dubbo.config.AbstractServiceConfig#register 为 false)，可通过Online命令上线服务

When using delay publishing function(com.alibaba.dubbo.config.AbstractServiceConfig#register setted as false), you can use “online” command to online the service 

```
//online all services
dubbo>online
OK

//online part of servies according to regular rules.
dubbo>online com.*
OK
```

改：由于没有进行JIT或相关资源的预热，当重启机器且线上QPS较高时，大量超时情况可能会发生。此时，可通过分批发布任务和逐渐加大流量来解决。
某台机器由于某种原因需要下线服务后，然后需要重新上线。

Common usage situations:
- Because there is no JIT or the related resources warm-up, when the machine is restarted and the online QPS is relatively high , a large amount of timeout situations may occur. At this time,the problem can be solved by distributing the wholesale service and increasing the traffic gradually.

- A machine needs to be back online after going offline due to some reasons.


### Offline service Command 

Offline command can be used if temporary offline service is needed when bug occurs. 

*中文更改建议：当故障发生时，如需要临时下线服务，可以使用Offline命令

```
//offline all service 
dubbo>offline
OK

//offline some services according to regular rules
dubbo>offline com.*
OK
```

### help command


```
//list all commands
dubbo>help

//list the specific use case of a command 
dubbo>help online
+--------------+----------------------------------------------------------------------------------+
| COMMAND NAME | online                                                                           |
+--------------+----------------------------------------------------------------------------------+
|      EXAMPLE | online dubbo                                                                     |
|              | online xx.xx.xxx.service                                                         |
+--------------+----------------------------------------------------------------------------------+

dubbo>
```


