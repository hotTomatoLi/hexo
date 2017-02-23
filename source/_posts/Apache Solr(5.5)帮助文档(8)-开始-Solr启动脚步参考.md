---
title: Apache Solr(5.5)帮助文档(8)-开始-Solr启动脚步参考
tags: Apache Solr(5.5)翻译
toc: true
---

>Solr Start Script Reference

Solr中"bin/solr"脚本，运行你启动，停止Solr，创建和删除集合或是Core，检查Solr运行状态和配置好的shards。可以在Solr安装目录下的`bin/`目录下找到这个脚本。`bin/solr`脚本通过提供简单的命令和选项使得Solr更容易使用，更快的实现一般目标。
本章节下面的标题与solr命令一致。对于每个命令，可用的选项都在示例中进行说明。
更多关于`bin/solr`使用的例子在整个帮助文档中都有，但是主要时在Running Solr章节和Getting started with SolrCloud。
- 启动和停止
  - 启动和重启
  - 停止
- 信息相关
  - 版本
  - 状态
  - 运行健康状态检查
- 集合和Core
  - 创建
  - 删除
- Zookeeper操作
  - 上传配置集合
  - 下载配置集合

### 启动和停止
#### 启动和重启
启动命令用来启动Solr，重启命令用来在Solr正在运行或以及被停止时重启Solr。
通过启动和重启命令的一些参数，可以使你的Solr运行在SolrCloud模式，或是使用示例项目配置，或是指定非默认端口，或是指向本地的一个Zookeeper。
```
bin/solr start [options]
```
```
bin/solr start -help
```
```
bin/solr restart [options]
```
```
bin/solr restart -help
```
当使用重启命令时，必须传递与启动时相同的参数。重启命令的后台运行时，会首先创建一个停止命令，从而在再次启动Solr前先停止运行中的Solr。当然，如果没有Solr节点在运行，重启命令会跳过停止这一步，直接到启动Solr。
##### 可用的参数
`bin/solr`脚本提供了很多参数，允许你按照一些常见的方式来启动服务器，例如改变监听端口。然而，特别是在初次接触时，绝大部分的默认配置已经足够使用。

| 参数        | 描述           | 示例  |
| ------------- |:-------------:|: -----:|
| -a "<string>"      | 采用额外的JVM参数启动Solr。<br>例如`-X`开头的一些参数参数。如果输入了以`-D`开头JVM参数，可以省略-a选项  | bin/solr start -a "-Xdebug<br>-Xrunjdwp:transport=dt_socket,<br>server=y,suspend=n,address=1044" |
| -cloud     | 按照SolrCloud模式启动Solr，<br>会启动在Solr中的嵌入的Zookeeper。<br> 参数可以简写为-c。<br> 如果已经启动了外部的Zkeeper，<br>想要用它替代嵌入的Zookeeper，<br>，需要传入-z参数。<br>更多细节，请查看后面的SolrCloud Mode章节。       |   bin/solr start -c|
| -d <dir>| 指定服务端的目录，默认为$SOLR_HOME/server。<br>一般很少会覆盖这个参数。<br>当在同一个主机下运行多个实例时，更常见的做法是使用统一的服务端目录，每个实例指定不同的Solr home目录（-s 选项|bin/solr start -d newSErverDir |
|-e <name> | 按照示例项目的配置启动Solr。这些示例是为了让你更快的熟悉Solr，或是尝试某个特定的特征而准备的。可选参数为：<br> cloud <br> techproducts <br> dih <br> shcemaless <br>更多细节可以查看Running with Example Configurations章节| bin/solr start -e schemaless|
|-f|在前台启动Solr；在使用-e参数时启动示例应用时不可以使用-f参数|bin/solr start -f|
|-h <hostname> | 按照指定的主机名词启动Solr。如果本参数没有设置，将会使用默认的"localhost"|bin/solr start -h search.mysolr.com|
|-m <memory>| 设置Solr启动时JVM堆大小的最小值(-Xms)和最大值(-Xmx)|bin/siolr start -m 1g|
|-nopromt| 在启动Solr时，禁止显示其它参数产生的提示。它带来的影响就是会在后台接受所有的默认设置。例如，当运行"cloud"示例时，一个交互式进程会允许你配置SolrCloud集群的参数。如果你希望按照默认值处理，只需要在请求上增加-nopromt参数。|bin/solr start -e cloud -noprompt|
|-p <port>|按照定义的端口启动Solr。如果参数未指定，会采用"8983"端口|bin/slr start -p 8655|
|-s <dir>|设置solr.solr.home系统变量。Solr会在该目录下创建core的目录。这个参数允许你在同一主机上创建多个Solr示例，并且复用-d参数设置的服务端目录。如果设置了本参数，指定的目录会包含solr.xml配置文件，除非该文件在Zookeeper上。本参数的默认值时`server/solr`。当运行示例程序时，这个参数会被忽略，是因为solr.solr.home变量依赖运行的示例程序。|bin/solr start -s newHome|
|-V|在启动时显示启动脚本输出的信息|bin/solr start -V|
|-z <zkHost>|按照定义的Zookeeper连接信息启动Solr。本参数仅仅会与-c参数一起使用，即在SolrCloud模式下。如果未提供本参数，Solr会启动嵌入的Zookeeper实例，用该实例来进行SolrCloud相关操作。|bin/solr start -c -z server1:2181,server2:2181|


为了强调默认参数如何工作的，我们可以通过下面两个命令来理解。下面这两个命令实际上时等价的。
```
bin/solr start
```

```
bin/solr start -h localhost -p 8983 -d server -s solr -m 512m
```
如果默认参数可以满足你的需求，就没有必要在启动时定义上述参数。

##### 设置JAVA参数
`bin/solr`脚本会将所有以-D开头的额外参数传递给JVM，这样就允许设置任意的Java系统参数。例如，如果要设置**soft-commit**频率为3秒，可以执行以下命令：
```
bin/solr start -Dsolr.autoSoftCommit.maxTime=3000
```

##### SolrCloud模式
-c参数和-cloud参数时等价的，
```
bin/solr start -c
```
```
bin/solr start -cloud
```
如果你指定了Zookeeper连接信息，例如"-z 192.168.1.4:2181"，那么Solr会连接到对应的Zookeeper，加入到该集群。如果在启动cloud模式时不指定-z参数，Solr会启动一个嵌入式的Zookeeper实例，端口号为当前Solr端口号加上1000。例如，如果Solr运行在端口8983，那么嵌入的Zookeeper将会在端口9983上运行。

重要信息：如果Zookeeper连接信息变更了根目录，如`localhost:2181/solr`，那么在你运行`bin/solr`脚本启动SolrCloud之前，需要加载 /solr的Zookeeper节点。这就需要使用`zkcli.sh`脚本，附带着启动的Solr脚本。
```
server/scripts/cloud-scripts/zkcli.sh -zkhost localhost:2181/solr -cmd bootstrap -solrhome server/solr
```
当运行在SolrCloud模式下时，交互进程会提示你选择一个使用的配置集。更多关于启动SolrCloud模式的信息，查看Getting Started with SolrCloud章节。

##### 运行实例配置
```
bin/solr start -e <name>
```
示例配置允许你通过一个现成的配置快速实现你希望从Solr中获得的内容。

每一个示例以**managed schema**形式启动Solr。该形式运行的Solr通过Schema API来修改schema，但是不允许手动修改schema.xml文件。如果你想要修改schema.xml文件，可以按照Managed Schema Definition in SolrConfig章节中的描述修改默认配置。
在下面的描述中，除了特别声明的，相应的示例都不允许SolrCloud模式或是无shcema模式。
下面是提供的示例：
- cloud：这个例子在一台机器上启动有1-4个节点的SolrCloud集群。当选择该模式，一个交互进程将会启动，指导你选择需要使用的初始化配置，示例集群的节点个数，端口号，需要创建的集合的名称。当使用本示例时，可以选择在$SOLR_HOME/server/solr/configsets目录下的任意一个配置。
- techproducts：本例启动了一个单机模式的Solr，相应的样例documents的schema文件在$SOLR_HOME/example/exampledocs目录中。配置文件在$SOLR_HOME/server/solr/configsets/sample_techproducts_configs目录下。
- dih：本例中，Solr为单机模式，并且启用了DataImportHandler(DIH)，不同的示例dataconfig.xml文件在启动前配置，这些示例配置包含了不同支持DIH的数据类别（数据库内容，email，RSS feeds等等）。配置集合时为DIH自定义的配置，可以在$SOLR_HOME/example/example-DIH/solr/conf目录下找到。更过关于DIH的信息，请查看Uploading Structured Data Stroe Data With the Data Import Handler。
- schemaless：本例启动了Solr的单机模式，使用的时**managed schema**。详细描述在Managed Schema Definition in SolrConfig中。本例中提供了非常少的预定义schema。在这种配置下，Solr将会运行在Schemaless模式，会在运行中创建字段，并根据输入的documents猜测字段的类别。配置集合位于$SOLR_HOME/server/solr/configsets/data_driver_schema_configs。

注意：在前台运行参数-f与-e参数不能一起使用，是由于启动脚本在启动Solr后需要执行其他的任务。

#### 停止
停止命令向正在运行的Solr服务器发送停止请求，从而可以正常关闭Solr。该命令会等待5秒钟来让Solr正常关闭，如果到时候还未关闭，会强制结束进程(kill -9)。
```
bin/solr stop [ptions]
```
```
bin/solr stop -help
```
##### 可用参数

|参数|描述|示例|
|:--:|:--:|:--:|
|-p<port>|停止在指定端口上运行的Solr。如果运行了多个Solr实例，或是运行了SolrCloud模式，或是要在不同的停止命令中指定不同的端口号，或是使用-all 参数|bin/solr stop -p 8983|
|-all|停止所有正在运行的有有效进程号的实例。|bin/slr stop -all|
|-k <key>|停止保护意外停止Solr的key，默认是"solrrocks"|bin/solr stop -k solrrocks|

### 信息相关
#### 版本
版本命令返回当前安装和正在运行的Solr版本。
```
bin/solr version
```

#### 状态
状态命令以JSON格式展示当前系统中正在运行的Solr节点的基本信息。状态命令使用SOLR_PID_DIR环境变量来定位Solr进程ID文件，从而找到运行的Solr实例；SOLR_PID_DIR环境变量默认是Solr安装目录的bin目录。
```
bin/solr status
```
输出结果将会包含集群中每个节点的状态，示例如下：
```
Found 2 Solr nodes:
Solr process 39920 running on port 7574
{
  "solr_home":"/Applications/Solr/solr-5.0.0/example/cloud/node2/solr/",
  "version":"5.0.0 1658469 - anshumgupta - 2015-02-09 09:54:36",
  "startTime":"2015-02-10T17:19:54.739Z",
  "uptime":"1 days, 23 hours, 55 minutes, 48 seconds",
  "memory":"77.2 MB (%15.7) of 490.7 MB",
  "cloud":{
    "ZooKeeper":"localhost:9865",
    "liveNodes":"2",
    "collections":"2"}}
Solr process 39827 running on port 8865
{
  "solr_home":"/Applications/Solr/solr-5.0.0/example/cloud/node1/solr/",
  "version":"5.0.0 1658469 - anshumgupta - 2015-02-09 09:54:36",
  "startTime":"2015-02-10T17:19:49.057Z",
  "uptime":"1 days, 23 hours, 55 minutes, 54 seconds",
  "memory":"94.2 MB (%19.2) of 490.7 MB",
  "cloud":{
    "ZooKeeper":"localhost:9865",
    "liveNodes":"2",
    "collections":"2"}}
```


#### 健康状态检测
当运行SolrCloud模式时，健康状态检测命令为每个集合生成JSON格式的健康状态报告。健康状态报告提供了一个集合中所有**shards**的每一个复制的状态信息，包括已经提交的documents数量和当前的状态。
```
bin/solr healthcheck [options]
bin/solr healthcheck -help
```
##### 可用参数
|参数|描述|示例|
|:--:|:--:|:--:|
|-c <collection>| 需要运行健康检测的集合名词（必填）|bin/solr healthcheck -c gettingstarted|
|-z <zkhost>|Zookeeper连接串，默认为"localhost:9983"，如果不是在8983端口运行Solr，需要指定相应的Zookeeper连接串。默认端口为Solr的端口加1000|bin/solr healthcheck  -z localhost:2181|

下面是一个健康检测的请求和响应示例。该Zookeeper采用的不是标准连接信息，当前有两个节点在运行。
```
$ bin/solr healthcheck -c gettingstarted -z localhost:9865
{
	"collection":"gettingstarted",
	"status":"healthy",
	"numDocs":0,
	"numShards":2,
	"shards":[
	{
	"shard":"shard1",
	"status":"healthy",
	"replicas":[
	{
	"name":"core_node1",
	"url":"http://10.0.1.10:8865/solr/gettingstarted_shard1_replica2/",
	"numDocs":0,
	"status":"active",
	"uptime":"2 days, 1 hours, 18 minutes, 48 seconds",
	"memory":"25.6 MB (%5.2) of 490.7 MB",
	"leader":true},
	{
	"name":"core_node4",
	"url":"http://10.0.1.10:7574/solr/gettingstarted_shard1_replica1/",
	"numDocs":0,
	"status":"active",
	"uptime":"2 days, 1 hours, 18 minutes, 42 seconds",
	"memory":"95.3 MB (%19.4) of 490.7 MB"}]},
	{
	"shard":"shard2",
	"status":"healthy",
	"replicas":[
	{
	"name":"core_node2",
	"url":"http://10.0.1.10:8865/solr/gettingstarted_shard2_replica2/",
	"numDocs":0,
	"status":"active",
	"uptime":"2 days, 1 hours, 18 minutes, 48 seconds",
	"memory":"25.8 MB (%5.3) of 490.7 MB"},
	{
	"name":"core_node3",
	"url":"http://10.0.1.10:7574/solr/gettingstarted_shard2_replica1/",
	"numDocs":0,
	"status":"active",
	"uptime":"2 days, 1 hours, 18 minutes, 42 seconds",
	"memory":"95.4 MB (%19.4) of 490.7 MB",
	"leader":true}]}]}
```

### Collections和Cores
bin/solr脚本可以帮助你创建新的collections（SolrCloud模式）或是cores（单机模式），或者删除collections。
#### 创建
注意：创建命令的用户权限
        当使用"create"命令，确保运行当前命令的用户与启动Solr的用户是同一个用户。如果使用的是UNIX/Linux安装脚本，将会创建一个名为solr的用户。如果Solr是以solr用户运行，但是用root用户创建core，那么Solr将不能覆盖由start脚本创建的目录。
        如果运行的是cloud模式，权限就不是个问题了。在cloud模式下，所有的配置都存储在Zookeeper中，创建脚本不需要创建目录或是复制配置文件。Solr自己会创建所有必须的目录。


创建命令会检测当前在运行的Solr模式（单机或是SolrCloud），然后根据模式创建core或是collection。
##### 可用参数
|参数|描述|示例|
|:--:|:--:|:--:|
|-c <name>|要创建的collection或是core的名字，必填。|bin/solr create -c mycollection|
|-d <confdir>|配置信息目录，默认值为data_driven_schema_configs。关于更多细节，查看Configuration Directories and SolrCloud章节|bin/solr create -d basic_config|
|-n <configName>|配置的名称。默认值是与core或是collection的名称一致|bin/solr create -n basic|
|-p <port>|创建命令对应的Solr实例的端口号，默认情况下，脚本会查找运行中的Solr实例，从而检测到端口号。当在同一台主机上运行多个单机Solr实例时，-p参数是非常有用的，可以指定想要在具体哪个实例上创建core|bin/solr crate -p 8983|
|-s <shards> <br> -shards|将collection划分成shards的个数，默认为1；只有当运行在SolrCloud模式下才生效。|bin/solr create -s 2|
|-rf <replicas> <br> -replicationFactor|collection中每个document的备份个数，默认是1（无备份）|bin/solr create -rf 2|

##### 配置目录和SolrCloud
在SolrCloud上创建collection之前，collection使用的配置目录需要上传到ZooKeeper。创建命令支持多种collections和配置目录工作的方式，你需要决定的是ZooKeeper中的配置目录师傅需要在多个collection之间共享。让我们通过几个例子来阐述SolrCloud中配置目录如何工作的。

首先，如果不提供-d或是-n参数，默认配置（$SOLR_HOME/server/solr/configsets/data_driver_schema_configs/conf）会上传到ZooKeeper，名称与collection一致。例如，`bin/solr create -c contacts`命令，将会把data_driver_schema_configs的配置上传到ZooKeeper中的/configs/contacts目录下。此时，如果创建另一个collection，命令脚本为`bin/solr create -c contacts2`，那么，data_driver_schema_configs目录会被复制到ZooKeeper的/configs/contacts2下。在contacts的collection上作的任何配置修改都不会影响contracts2的collection。简单来书，默认情况下，会为你创建的每一个collection复制唯一的配置目录。

你可以通过-n参数，修改上传到ZooKeeper的配置目录的名称。例如，`bin/solr create -c logs -d basic_config -n basic`将会将server/solr/configsets/basic_configs/conf目录上传到ZooKeeper下的/configs/basic。

通过`-d`参数，我们使用指定的配置目录替换了采用默认配置。在`server/solr/configsets`目录下，Solr提供了多个内置的配置文件。当然，你也可以通过`-d`参数指定自己的配置文件目录。例如，`bin/solr create -c mycoll -d /tmp/myconfigs`命令，将会把`/tmp/myconfigs`上传到Zookeeper下的`/configs/mycoll `。重申一下，配置文件目录的名称与collection的名称保持一致，除非使用`-n`参数修改该值。

其他的collection可以通过使用`-n`参数指定需要共享的配置。例如，下面的命令将会创建一个使用先前创建的basic 配置的新collection。`bin/solr create -c logs2 -n basic`。

##### 数据驱动schema和共享配置
`data_driven_schema_configs `模式下的schema，会在数据索引时产生变化。因此，我们不建议在多个collection之间共享数据驱动的配置，除非你十分确定当数据索引进任意一个collection时，所有的collection都需要产生一样的变化。

#### 删除
删除命令会检测正在运行的Solr（单机模式或是SolrCloud模式），然后根据不同的情况选择删除指定的core（单机模式）或是指定的collection（SolrCloud模式）.。
```
bin/solr delete [options]
bin/solr delete -help
```
如果运行在SolrCloud模式下，删除命令会检查要删除的collection使用的配置目录是否被其他的collection所使用。如果不是，那么对应的配置目录也会从ZooKeeper下删除。例如，如果你通过`bin/solr create -c contacts`创建了一个collection，那么删除命令`bin/solr delete -c contacts`将会检查`/configs/contacts `配置目录是否被其他的collection使用。如果是否，那么`/configs/contacts`目录会从ZooKeeper下移除。

##### 可用参数
|参数|描述|示例|
|:--:|:--:|:---:|
|-c <name>|需要删除的core或是collection，必填|bin/solr delete -c mycoll|
|-deleteConfig <true or false>|是否从ZooKeeper下删除配置目录，默认为true。如果该目录被其他collection使用，那么即使使用-deleteConfig true参数，也不会被删除。|bin/solr delete -deleteConfig false|
|-p <port>|需要执行删除命令的Solr实例的端口号。默认情况下，脚本通过查找运行中的Solr实例，来检测端口号。当在一台主机上运行多个单机Solr实例时，通过本参数来指定需要从哪个实例上删除core|bin/solr delete -p 8983|

### ZooKeeper操作
`bin/solr`脚本运行通过特定操作来控制ZooKeeper，这些操作只能再SolrCloud下执行。
```
bin/solr zk [options]
bin/solr zk -help
```
注意：在通过上述命令初始化ZooKeeper和Solr节点之前，Solr必须至少启动过一次。一旦ZooKeeper初始化完成，Solr就不需要任意节点来运行这些命令，可以但多运行。

#### 上传配置集
通过ZooKeeper子命令可以将预先配置好的或是自定义配置集上传到ZooKeeper。

##### 可选参数（都是必填参数）
|参数|描述|示例|
|:--:|:--:|:---:|
|-upconfig|从本地文件系统上传配置集到ZooKeeper|-upconfig|
|-n <name>|在ZooKeeper中的配置集的名称。这个命令会将配置集上传到ZooKeeper节点上的"configs"下指定名称的目录下。<br>可以通过云screens的管理界面查看所有上传的配置，通过Cloud->tree->config来查看。<br> 如果指定一个已经存在的配置，那么会进行覆盖操作。|-n myconfig|
|-d <configsetdir>|需要上传的配置集的路径。在路径下应该有"conf"文件夹，里面包含了solrconfig.xml等。<br>如果仅提供了一个目录名字，那么会在`$SOLR_HOME/server/solr/configsets`路径下检查该目录，因此最好提供绝对路径|-d directory_under_configsets <br> -d /absolute/path/to/configset/source|
|-z <zkHost>|ZooKeeper连接串|-z 123.321.23.43:2181|

一个带有这些参数的示例命令时
```
bin/solr zk -upconfig -z 111.222.333.444:2181 -n mynewconfig -d /path/to/configset
```
上述命令不会让变更立即生效，它仅仅上传配置集到ZooKeeper。可以通过Collection API来触发一个重载命令，使得配置生效。

#### 下载配置集
通过ZooKeeper子命令可以从ZooKeeper下载一个配置集。

##### 可选参数（都是必填参数）
|参数|描述|示例|
|:--:|:--:|:---:|
|-downconfig|从ZooKeeper下载一个配置集到本地文件系统。| -downconfig|
|-n <name>|要下载的配置集的名称。管理界面>>Cloud>>tree>>configs 展示了所有的配置集。| -n myconfig|
|-d<configsetdir>|下载的配置集的保存路径。如果仅提供目录名，将会在$SOLR_HOME/server/solr/configsets路径下保存目录；也可以提供一个绝对路径。在任意一种情况下，如果目的地址已经存在相应的配置，那么该目录会被覆盖。| -d directory_under_configsets -d /absolute/path/to/configset/destination|
|-z <zkHost>|ZooKeeper连接串|-z 123.321.23.43:2181|
一个带有这些参数的示例命令时
```
bin/solr zk -downconfig -z 111.222.333.444:2181 -n mynewconfig -d
/path/to/configset
```
最好的方式是在版本控制系统中保存你的配置文件，那样的话，下载命令将基本不会使用。