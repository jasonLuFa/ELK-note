# Table of Contents

- [Table of Contents](#table-of-contents)
  - [基本架構](#基本架構)
  - [Dev Tools](#dev-tools)
  - [Node Role](#node-role)

## 基本架構

- Node：可以是虛擬/實體機或是 Container 用來存放多個資料，通常開發時會使用一個 Node
- Cluster：是一個或多個 Node 的集合
- Document：資料存放方式，也就是 json 格式
- Index / Indices：是一個或多個 Document 的集合，用來切分邏輯以利於查詢
- Sharding：將 Index 分成多個 Shard，以利於水平擴展
  - 如果預計會有幾百萬個 Doucment 才會需要新增 Shard
  - 有相應的 API 來新增/減少 Shard 數量
  - Shard 數量會根據 Node 數量/容量、indices 數量/大小、查詢的數量等因素決定
- Replication：將 Index 分成多個 Replica，以利於備份
  - replication 運作方式是複製 Shard(Primary Shard)，該副本稱為 Replica Shard，且其有所有 Primary Shard 的功能，Primary Shard 好 Replica Shard 稱為 Replication Group（預設情況下 index 建立時會建立 1 個 Primary Shard 和 1 個 Replica Shard）
  - 提高==可用性==，Primary Shard 和 Replica Shard 通常會在不同機器上(不同 nodes)以避免資料不見
  - 提高==吞吐量==，如果該機器不會很忙碌的情況，多個 Replica 會因為機器多核並行處理更多該 index 的請求
- Snapshots：備份某一時間點的資料，以利於 rollback。例如要改變資料前可以 snapshot，當資料改壞時，還可以 rollback 回去

## Dev Tools

- `GET /_cluster/health` : 提供有關整個 Elasticsearch 集群的健康信息 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-info.html))
- `GET /_cat/nodes?v` : 傳回叢集中節點的清單 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html))
  - `?v` 代表 verbose
- `Get /_cat/indices?v` : 傳回叢集中的索引的清單 ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-indices.html))
- `Get /_cat/shards?v` : 傳回分片的清單（包包括 primary shard 和 replica shards） ([參考](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-shards.html))

## Node Role

- 通常用預設的就好，不用特別去改角色，除非在很大的集群中
- 配置在 `elasticsearch.yml` 中:
  - 主要的角色
    - Master Node：管理集群 (Configure: node.master: true | false)
    - Data Node：儲存資料 (Configure: node.data: true | false)
  - 其他
    - Ingest Node：用來預處理資料 (Configure: node.ingest: true | false)
    - ...