---
layout: default
title: Filebeat7自定义index的一个坑
---

最近在尝试使用 filebeat 收集日志，也没有明确的目的，版本是7.3.2，对应的 `elsearch` 版本也是7.3.2

安装过程的坑不在此文，会单列文章分析，直接说一个大坑。

filebeat 收集日志后，在 kibana 中查看时，索引名为 `filebeat-7.3.2-2019.09.18`，这种命名非常不利于分析日志，于是找到官方文档，

> Filebeat uses time series indices, by default, when index lifecycle management is disabled or unsupported. The indices are named filebeat-7.3.2-yyyy.MM.dd, where yyyy.MM.dd is the date when the events were indexed. To use a different name, you set the index option in the Elasticsearch output. The value that you specify should include the root name of the index plus version and date information. You also need to configure the setup.template.name and setup.template.pattern options to match the new name. For example:

```
 output.elasticsearch.index: "customname-%{[agent.version]}-%{+yyyy.MM.dd}"
 setup.template.name: "customname"
 setup.template.pattern: "customname-*"
```


原封不动地帖到我们的 `filebeat.yml` 中，但是发现并不能修改索引名为 customname，最开始，还以为是我对 yml 文件格式理解有误，于是各种修改这三行脚本，发现始终不能改动索引名。找了很多文章，说的都是是一样的东西毫无新意。

在挣扎了几天下班时间之后，痛定思痛，把 filebeat 启动的日志拿出来看，一行行看，发现一行很奇怪的东西 `Set output.elasticsearch.index to 'filebeat-7.3.2' as ILM is enabled.` 翻译过来就是 `因为ILM 为生效状态，设置 output.elasticsearch.index 为filebeat-7.3.2`，豁然开朗，是不是可能是这个 ilm 的锅呢，google 之后发现了各位大神有同样经历，感谢[原文](https://iminto.github.io/post/filebeat修改index的一个坑/)作者。

按原作者经历，这是 elk 官方文档不清楚，添加设置 `setup.ilm.enabled: false`，重启，检查 kibana，发现，成了，索引名称已经修改成我们配置的 `customname-%{[agent.version]}-%{+yyyy.MM.dd}`，再次感谢作者。

记录下原文的核心

```
{filebeat-7.1.1 {now/d}-000001} 这个名字总是会覆盖我自己的配置。反复尝试，觉得是 ILM 这个东西在作梗，于是试着搜索了下“filebeat ILM is enabled”，发现了这个issue ，有不少人踩坑了。提出issue的人也指出了文档没有说清楚。
   
   指向了这个官方文档：https://www.elastic.co/guide/en/beats/filebeat/current/ilm.html
```

> 后记，此次事件折腾了我几天的下班时间，时间应该有4-5个小时，总结就是要看日志，如果这次提前看了 filebeat 的日志，就能早些发现问题的核心点。

> 此次事件对我整个发展来说也有积极一面：了解了filebeat 不是那么难搞，工具而已，作者们都是是尽可能让我们使用者简单


![关注我](http://note.youdao.com/yws/api/personal/file/D4F33BFCDB0846F09597A867DE622CF9?method=download&shareKey=c7d1dd0af3045032bcd2f61952494356)

我知道你
<font color='red'>在看</font>