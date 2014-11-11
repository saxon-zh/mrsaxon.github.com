---
layout: post
title: "Linux_crontab执行脚本中文乱码,手动执行正常"
description: ""
category: "Linux"
tags: ["Linux", "crontab", "shell", "中文乱码"]
---
{% include JB/setup %}


最近在线上产品中部署了一套操作系统版本数据采集工具，工具会将采集到的数据在按照服务器所属业务进行分级汇总，在推送到HTTP接口存储到DB

但是部署到线上之后的第二天（因为采集是按照天为单位进行），做数据验证发现推送到服务器上的数据存在部分级别内容为空   

最终排查发现数据中只有中文数据会为空，其他则不会   

而且这种现象只会发生在crontab执行的情况下，手动执行脚本做数据推送则不会有问题  

那么自然而然会想到语言编码的问题分析上，其实crontab执行时已经不在是用户环境，那么很多用户中的配置就会失效，比如用户配置了UTF8的编码，但是crontab执行的时候就可能会变成ISO的编码   

所以要想解决这个问题，那么就在crontab执行脚本的时候保持和用户环境配置一直即可  

在用户环境下执行下面的命令：   

{% highlight php %}
echo $LANG
{% endhighlight %}   

将输出得到的内容（例如：zh_CN.UTF-8），那么就在你执行的crontab脚本中增加下面的一行命令即可：

{% highlight php %}
export LANG=en_US.UTF-8
{% endhighlight %}   

最终就彻底解决了中文无法正常推送的问题