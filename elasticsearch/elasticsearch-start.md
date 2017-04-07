
Elasticsearch is a highly scalable open-source full-text search and analytics engine.

Basic Concepts

Near Realtime (NRT)

Cluster

Node

Index

Type

Document


Shards & Replicas



安装 Elasticsearch

Elasticsearch requires at least Java 8. 
Specifically as of this writing, it is recommended that you use the Oracle JDK version 1.8.0_73. 

$ java -version
java version "1.8.0"
Java(TM) SE Runtime Environment (build 1.8.0-b132)
Java HotSpot(TM) 64-Bit Server VM (build 25.0-b70, mixed mode)


```
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.0.tar.gz
$ tar zxf elasticsearch-5.3.0.tar.gz 
$ cd elasticsearch-5.3.0 
$ bin/elasticsearch

$ curl http://localhost:9200/
{
    name: "xdkVW67",
    cluster_name: "elasticsearch",
    cluster_uuid: "To-C3m1kQpyMLX8suLxK7g",
    version: {
        number: "5.3.0",
        build_hash: "3adb13b",
        build_date: "2017-03-23T03:31:50.652Z",
        build_snapshot: false,
        lucene_version: "6.4.1"
    },
    tagline: "You Know, for Search"
}
```

REST API 

Among the few things that can be done with the API are as follows:

Check your cluster, node, and index health, status, and statistics
Administer your cluster, node, and index data and metadata
Perform CRUD (Create, Read, Update, and Delete) and search operations against your indexes
Execute advanced search operations such as paging, sorting, filtering, scripting, aggregations, and many others

* 参数 v, 可以显示结果的标题栏   
* 参数 pretty, 可以美观的形式打印出JSON响应   
* 参数 format, 可以选择响应的格式，目前支持text, yaml, json三种 
* 参数 s, 可以选择响应的格式，目前支持text, yaml, json三种 

```
$ curl 'localhost:9200/_cat/master'
xdkVW67ARQiukQf9nVY3PA 127.0.0.1 127.0.0.1 xdkVW67
```

```
$ curl 'localhost:9200/_cat/master?v'
id                     host      ip        node
xdkVW67ARQiukQf9nVY3PA 127.0.0.1 127.0.0.1 xdkVW67
```

```
// 默认: format=text
$ curl 'localhost:9200/_cat/master?v'
id                     host      ip        node
xdkVW67ARQiukQf9nVY3PA 127.0.0.1 127.0.0.1 xdkVW67

// format=yaml
$ curl 'localhost:9200/_cat/master?v&format=yaml'
---
- id: "xdkVW67ARQiukQf9nVY3PA"
  host: "127.0.0.1"
  ip: "127.0.0.1"
  node: "xdkVW67"

// format=json
$ curl 'localhost:9200/_cat/master?v&format=json'
[{"id":"xdkVW67ARQiukQf9nVY3PA","host":"127.0.0.1","ip":"127.0.0.1","node":"xdkVW67"}]%                                                                   

$ curl 'localhost:9200/_cat/master?v&format=json&pretty'
[
  {
    "id" : "xdkVW67ARQiukQf9nVY3PA",
    "host" : "127.0.0.1",
    "ip" : "127.0.0.1",
    "node" : "xdkVW67"
  }
]
```

$ curl 'localhost:9200/_cat/master?help'
id   |   | node id
host | h | host name
ip   |   | ip address
node | n | node name


For example, with a sort string s=column1,column2:desc,column3, the table will be sorted in ascending order by column1, in descending order by column2, and in ascending order by column3.


curl 'http://localhost:9200/_cat/health?v'

```
curl 'http://localhost:9200/_cat/health?v'
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1491441171 09:12:51  elasticsearch green           1         1      0   0    0    0        0             0                  -                100.0%
```

集群的名字是“elasticsearch”，状态是绿色, 正常。
当我们询问集群状态的时候，我们要么得到绿色、黄色或红色。
在 Elasticsearch 集群中可以监控统计很多信息，其中最重要的就是：集群健康(cluster health)。它的 status 有 green、yellow、red 三种；
三种颜色分别代表：
* green： 所有主分片和从分片都可用
* yellow： 所有主分片可用，但存在不可用的从分片
* red： 存在不可用的主要分片


后去节集群中的节点列表:

```
curl 'http://localhost:9200/_cat/nodes?v'
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
127.0.0.1           20         100   7    1.68                  mdi       *      xdkVW67
```


列出所有的索引
```
curl 'localhost:9200/_cat/indices?v'
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
```
这个结果意味着，在我们的集群中，我们没有任何索引。

现在我们创建一个叫做 "helloworld" 的索引，然后再列出所有的索引：

```
// 注意索引名称须为小写字母，否则会报错“Invalid index name [HelloWorld], must be lowercase”
$ curl -XPUT 'localhost:9200/HelloWorld?pretty'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "invalid_index_name_exception",
        "reason" : "Invalid index name [HelloWorld], must be lowercase",
        "index_uuid" : "_na_",
        "index" : "HelloWorld"
      }
    ],
    "type" : "invalid_index_name_exception",
    "reason" : "Invalid index name [HelloWorld], must be lowercase",
    "index_uuid" : "_na_",
    "index" : "HelloWorld"
  },
  "status" : 400
}

// 创建一个名为 helloworld 的索引
$ curl -XPUT 'localhost:9200/helloworld?pretty'
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}

// 列出所有索引 
$ curl 'localhost:9200/_cat/indices?v'
health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   helloworld nE6isWA2TaCdePngczJeJw   5   1          0            0       650b           650b
```

黄色健康标签， 状态 open 


让我们将一个简单的客户文档索引到 helloworld 索引、“external”类型中，这个文档的ID是1，操作如下：
        
$curl -XPUT 'localhost:9200/helloworld/external/1?pretty' -d '{"name":"wangyt"}'
{
  "_index" : "helloworld",
  "_type" : "external",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "created" : true
}

```
$ curl 'localhost:9200/_cat/indices?v'
health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   helloworld nE6isWA2TaCdePngczJeJw   5   1          1            0      3.7kb          3.7kb
```

有5个主分片和1份复制（都是默认值），其中包含0个文档。
docs.count 1
store.size 3.7kb 
pri.store.size 3.7kb

列出所有索引，使用json格式 
$ curl 'localhost:9200/_cat/indices?format=json&pretty&v'
[
  {
    "health" : "yellow",
    "status" : "open",
    "index" : "helloworld",
    "uuid" : "nE6isWA2TaCdePngczJeJw",
    "pri" : "5",
    "rep" : "1",
    "docs.count" : "1",
    "docs.deleted" : "0",
    "store.size" : "3.8kb",
    "pri.store.size" : "3.8kb"
  }
]

$ curl -XPUT 'localhost:9200/hello.search?format=json&pretty'
{
  "acknowledged" : true,
  "shards_acknowledged" : true
}

$ curl 'localhost:9200/_cat/indices?pretty'
health status index        uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   hello.search HByjj1YOSwWed-83m9SmYw   5   1          0            0       650b           650b
yellow open   helloworld   nE6isWA2TaCdePngczJeJw   5   1          1            0      3.8kb          3.8kb


$ curl -XPUT 'localhost:9200/hello.search/external/1?pretty' -d '{"name":"Mr.Wang"}'
$ curl -XPUT 'localhost:9200/hello.search/external/2?pretty' -d '{"name":"Mr.Yong"}'
$ curl -XPUT 'localhost:9200/hello.search/external/3?pretty' -d '{"name":"Mr.Tao"}'

$ curl 'localhost:9200/_cat/indices?pretty'
yellow open hello.search HByjj1YOSwWed-83m9SmYw 5 1 4 0 13.2kb 13.2kb
yellow open helloworld   nE6isWA2TaCdePngczJeJw 5 1 1 0  3.8kb  3.8kb

$ curl 'localhost:9200/_cat/indices?pretty&v'
health status index        uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   hello.search HByjj1YOSwWed-83m9SmYw   5   1          4            0     13.2kb         13.2kb
yellow open   helloworld   nE6isWA2TaCdePngczJeJw   5   1          1            0      3.8kb          3.8kb

删除API，可以根据特定的ID删除文档。

$ curl -XDELETE 'http://localhost:9200/hello.search/external/2?pretty'
{
  "found" : true,
  "_index" : "hello.search",
  "_type" : "external",
  "_id" : "2",
  "_version" : 3,
  "result" : "deleted",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  }
}

// 删除 hello.search 全部索引
$ curl -XDELETE 'http://localhost:9200/hello.search?pretty'
{
  "acknowledged" : true
}

在添加索引的时候，ID部分是可选的。
如果不指定，Elasticsearch将产生一个随机的ID来索引这个文档。 

以下的例子展示了怎样在没有指定ID的情况下来索引一个文档：
 
curl -XPOST 'localhost:9200/hello.search/external?pretty' -d '{"name":"Tom.Cat.Dog.Fish"}'
{
  "_index" : "hello.search",
  "_type" : "external",
  "_id" : "AVtBBCwvPvrk6DMcZhkn",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "created" : true
}
注意，在上面的实例中，由于我们没有指定一个ID，使用的是 POST 而不是 PUT， 生成了随机的ID：AVtBBCwvPvrk6DMcZhkn 。

搜索的REST　API可以通过_search端点来访问。下面这个例子返回bank索引中的所有的文档：

curl 'localhost:9200/hello.search/_search?q=*&pretty'

curl 'localhost:9200/hello.search/_search?q=name:Mr.Yong&pretty'
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 0.2876821,
    "hits" : [
      {
        "_index" : "hello.search",
        "_type" : "external",
        "_id" : "2",
        "_score" : 0.2876821,
        "_source" : {
          "name" : "Mr.Yong"
        }
      }
    ]
  }
}


curl 'localhost:9200/hello.search/_search?q=name:Mr*&pretty'
{
  "took" : 20,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 3,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "hello.search",
        "_type" : "external",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "name" : "Mr.Yong"
        }
      },
      {
        "_index" : "hello.search",
        "_type" : "external",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "name" : "Mr.Wang"
        }
      },
      {
        "_index" : "hello.search",
        "_type" : "external",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "name" : "Mr.Tao"
        }
      }
    ]
  }
}

   对于这个响应，我们看到了以下的部分：
          - took —— Elasticsearch执行这个搜索的耗时，以毫秒为单位
          - timed_out —— 指明这个搜索是否超时
          - _shards —— 指出多少个分片被搜索了，同时也指出了成功/失败的被搜索的shards的数量
          - hits —— 搜索结果
          - hits.total —— 能够匹配我们查询标准的文档的总数目
          - hits.hits —— 真正的搜索结果数据（默认只显示前10个文档）
          - _score和max_score —— 现在先忽略这些字段


curl -XPOST 'localhost:9200/hello.search/_search?pretty' -d '{"query":{"match_all":{}}}'

curl -XPOST 'localhost:9200/hello.search/_search?pretty' -d '{"query":{"match_all":{}}}'
curl -XGET 'localhost:9200/hello.search/_search?pretty' -d '
    {
      "query": { 
        "match": {
            "name":"Mr."
        } 
      }
    }'
