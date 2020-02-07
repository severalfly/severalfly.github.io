---
layout: default
title: 2020-02-07-Elasticsearch学习（2）--ES配置加载.md
---
上节，我们学习到了如何从源码启动 elasticsearch，从现在开始我们来深入阅读源码

本节来简单看一下 es 是如何加载配置的。

一，ignorePrefixes 
跟踪了代码，发现settings 来源于 `org.elasticsearch.node.internal.InternalSettingsPreparer`，首先定义了一个忽略的配置前缀 `String[] ignorePrefixes = new String[]{"es.default.", "elasticsearch.default."};`，推测是为了防止重复配置，尽可能使用项目内的配置项，而忽略系统的配置项。

二，useSystemProperties
定义了 `useSystemProperties` 是否使用系统配置，启动项目时，传入的配置为空，也就是说此值为 true ，也即使用系统配置里面前缀为 `elasticsearch.default.`,`elasticsearch.`,`es.default.`,`es.`的配置项。

三，loadConfigSettings
构造方法里面，传入了这个选项，是否加载配置文件。首次启动时，当然是加载啦！根据 useSystemProperties 判断是否加载系统内部的配置选项，同时会设置 `loadFromEnv`，如果系统已经有配置，则 loadFromEnv 为 false ，即，不从配置文件加载。否则会从 elasticsearch.yml，elasticsearch.json， elasticsearch.properties 这三个文件中依次加载

四，force
去除 force. 开头的配置的前缀

五，name.txt
从此文件中，加载一个名字，具体用途，还不清楚。先占坑

六，cluster name
配置默认的 `cluster name`，如果在配置文件中已经存在，则不使用默认配置

七，添加日志目录


最后，希望本文对你有所启发，早日开始elasticSearch 的源码阅读
=