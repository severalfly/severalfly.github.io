---
layout: default
title: Elasticsearch学习（3）--ES启动任务
---
上节，我们学习到了 elasticsearch 启动时加载的配置文件，以及加载顺序。  
本节我们来了解一下 es 在启动时加载了哪些异步任务，本节代码在 `org.elasticsearch.node.internal.InternalNode.start`  

## 如何启动一个异步任务

首先我们看一下任务是如何启动的，举一个例子，`injector.getInstance(IndicesService.class).start();` `IndicesService` 的唯一实现类为 `InternalIndicesService`，且 `InternalIndicesService` 又继承自 `AbstractLifecycleComponent`，同时 `AbstractLifecycleComponent` 有一个start 方法
![abc-w150](../../../images/elk/es/20200208/597BE0B4-7AA3-4D0D-8598-F7F21BED07AE.png)

<img src="../../../images/elk/es/20200208/597BE0B4-7AA3-4D0D-8598-F7F21BED07AE.png" width = "80%" />  
可以发现，其内部实际上调用的是 doStart() 方法，只不过是添加了一些控制，保证不重复启动之类的代码


直接摘抄代码，如下，我们做个详细的解释，见注释
```
for (Class<? extends LifecycleComponent> plugin : pluginsService.services()) {
    injector.getInstance(plugin).start(); // 启动插件需要的任务
}

injector.getInstance(IndicesService.class).start();
injector.getInstance(IndexingMemoryController.class).start();
injector.getInstance(IndicesClusterStateService.class).start();
injector.getInstance(IndicesTTLService.class).start();
injector.getInstance(RiversManager.class).start();
injector.getInstance(ClusterService.class).start();
injector.getInstance(RoutingService.class).start();
injector.getInstance(SearchService.class).start();
injector.getInstance(MonitorService.class).start();
injector.getInstance(RestController.class).start();
injector.getInstance(TransportService.class).start();
DiscoveryService discoService = injector.getInstance(DiscoveryService.class).start();

// gateway should start after disco, so it can try and recovery from gateway on "start"
injector.getInstance(GatewayService.class).start();

if (settings.getAsBoolean("http.enabled", true)) {
    injector.getInstance(HttpServer.class).start();
}
injector.getInstance(BulkUdpService.class).start();
injector.getInstance(ResourceWatcherService.class).start();
injector.getInstance(TribeService.class).start();
```



最后，希望本文对你有所启发，早日开始elasticSearch 的源码阅读
=
了解更多请关注我的公众号
![](http://note.youdao.com/yws/api/personal/file/D4F33BFCDB0846F09597A867DE622CF9?method=download&shareKey=c7d1dd0af3045032bcd2f61952494356)