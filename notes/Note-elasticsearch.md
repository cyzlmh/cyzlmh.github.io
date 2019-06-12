---
layout: page
title: Elasticsearch
category: note
tags: Others
---

* content

{:toc}

## Basic

``` json
GET /_cat/indices

GET /_cat/health

GET /_cat/nodes
```

create index with mappings

``` json
PUT /index_name
{
  "mappings": {
    "_doc": {
      "properties": {
        "field_keyword": {"type": "keyword"},
        "field_text": {"type": "text"},
        "field_date1": {"type": "date", "format": "epoch_second"},
        "field_date2": {"type": "date", "format": "yyyyMMdd"},
        "field_location": {"type": "geo_point"},
        "field_int": {"type": "integer"},
        "field_bool": {"type": "boolean"},
        "field_float": {"type": "float"},
        "field_double": {"type": "double"},
        "field_alias": {"type": "alias", "path": "poi_id"}
      }
    }
  },
  "settings": {
    "number_of_replicas": 0
  },
  "aliases": {"the_alias": {}}
}
```

insert row

``` json
POST /index/_doc
{
  "field1": "data1",
  "field2": "data2"
}
```

create alias

``` json
POST /_aliases
{
  "actions":[
    {"remove": {
      "index": "old_index",
      "alias": "index_alias"
    }},
    {"add": {
      "index": "new_index",
      "alias": "index_alias"
    }}
  ]
}

# Only one index per alias can be assigned to be the write index at a time
# If no write index is specified and there are multiple indices referenced by an alias, then writes will not be allowed.

POST /_aliases
{
  "actions":[
    {"add": {
      "index": "new_index",
      "alias": "index_alias",
      "is_write_index": true
    }},
    {"add": {
      "index": "old_index",
      "alias": "index_alias",
      "is_write_index": false
    }}
  ]
}
```

query

``` json
POST /index/_search
{
  "size": 10,
    "bool": {
      "must": [
        {"term": {"field1": "keyword1"}},
        {"term": {"field2": "keyword2"}}
      ]
    }
  }
}

POST /index/_search
{
  "query": {
    "bool": {
      "must": [
        {"term": {"field1": "keyword1"}},
        {"term": {"field2": "keyword2"}}
      ]
    }
  },
  "aggs": {
    "name": {"sum": {"field": "field1"}}
  }
}

POST /index/_search
{
  "query": {
    "bool": {
      "must": [
        {"term": {"field1": "keyword1"}},
        {"term": {"field2": "keyword2"}}
      ]
    }
  },
  "aggs": {
    "by_term": {
      "terms": {"field": "filed3", "size": "10"},
      "aggs": {"sum_per_field": {"sum":{"field": "field4"}}}
    }
  }
}

POST /index/_search
{
  "query": {
    "bool": {
      "must": [
        {"term": {"field1": "keyword1"}},
        {"term": {"field2": "keyword2"}}
      ]
    }
  },
  "aggs": {
    "by_date": {
      "date_histogram": {"field": "ds", "interval": "day"},
      "aggs": {"sum_per_day": {"sum":{"field": "field3"}}}
    }
  }
}
```

range query

``` json
POST /index/_search
{
  "query": {
    "bool": {
      "filter": [
        {"range": {"timestamp": {"gt": "now-4d/d", "lte": "now-3d/d"}}}
      ]
    }
  }
}
```

## Template of Common Task

在ES中建立索引并定义类型

``` json
PUT /index_name
{
  "mappings": {
    "_doc": {
      "properties": {
        "field1": {"type": "keyword"},
        "field2": {"type": "integer"}
      }
    }
  },
  "settings": {
    "number_of_replicas": 0
  }
}
```

在Hive中创建外部表

``` sql
add jar hdfs:///data/script/poseidon/yzchen/elasticsearch-hadoop-6.4.2/dist/elasticsearch-hadoop-6.4.2.jar;
CREATE EXTERNAL TABLE index_name_to_elk (
    field1 string,
    field2 int
    )
STORED BY 'org.elasticsearch.hadoop.hive.EsStorageHandler'
TBLPROPERTIES('es.resource' = 'index_name/_doc',
    'es.index.auto.create' = 'false',
    'es.nodes' = 'http://9.24.140.147,http://9.24.140.148,http://9.24.140.149,http://9.24.140.150,http://9.24.140.152',
    'es.port'='9200'
);
```

导入数据

``` sql
insert overwrite table index_name_to_elk
select field1, field2
from index_name
```