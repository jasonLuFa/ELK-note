# Elasticsearch Note

## Dev Tools

- `GET /_cluster/health` : 提供有關整個 Elasticsearch 集群的健康信息 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-info.html))
- `GET /_cat/nodes?v` : 傳回叢集中節點的清單 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html))
  - `?v` 代表 verbose
- `Get /_cat/indices?v` : 傳回叢集中的索引的清單 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-indices.html))