# Elasticsearch - Basic Concepts

### Basic Concepts

- real-time distributed and open source full-text search and analytics engine
- developed by Shay Banon and published in 2010
- REST APIs, Java, 
- helps to explore very large amount of data at very high speed
- features:
  - scalable up to petabytes of structured and unstructured data.
  - a replacement of document stores like MongoDB and RavenDB.
  - uses denormalization to improve the search performance.

### Key Concepts

- **Node** − a single running instance of Elasticsearch.
- **Cluster** − a collection of nodes. It provides collective indexing and search capabilities across all the nodes for entire data.
- **Index** − a collection of different type of documents and document properties. Index also uses the concept of shards to improve the performance. 
- **Type/Mapping** − a collection of documents sharing a set of common fields present in the same index.

- **Document** − a collection of fields in a specific manner defined in JSON. Every document belongs to a type and resides inside an index. Every document is associated with a unique identifier, called the UID.
- **Shard** − Indexes are horizontally subdivided into shards. This means each shard contains all the properties of document, but contains less number of JSON objects than index. The horizontal separation makes shard an independent node, which can be store in any node. Primary shard is the original horizontal part of an index and then these primary shards are replicated into replica shards.
- **Replicas** − ElasticSearch allows a user to create replicas of their indexes and shards. Replication helps in increasing the availability of data and improves the performance of searching by carrying out a parallel search operation in these replicas.

### Elasticsearch – Advantages/ Disadvantages

- (+) real time; distributed; scalable in large org; backups are easy using gateway; multi-tenancy is very easy; JSON; support textual structured and unstructured data;
- (-) multi-language support; 

### Comparison between ES and RDBMS

|     ES      | **RDBMS** |
| :---------: | :-------: |
|    Index    | Database  |
|    Shard    |   Shard   |
|   Mapping   |   Table   |
|    Field    |   Field   |
| JSON Object |   Tuple   |

### Install 

```bash
λ ~/ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - 
λ ~/ echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
deb https://artifacts.elastic.co/packages/6.x/apt stable main
λ ~/ sudo apt update
λ ~/ sudo apt install elasticsearch
λ ~/ sudo nano /etc/elasticsearch/elasticsearch.yml
# and set network.host to localhost and port to 9200 
λ ~/ sudo systemctl start elasticsearch
λ ~/ sudo systemctl enable elasticsearch
Synchronizing state of elasticsearch.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable elasticsearch
Created symlink /etc/systemd/system/multi-user.target.wants/elasticsearch.service → /usr/lib/systemd/system/elasticsearch.service.
λ ~/ curl -X GET "localhost:9200"
{
  "name" : "-e28n1-",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "-b_c43bMQQiaL8Wl7UdNQQ",
  "version" : {
    "number" : "6.6.0",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "a9861f4",
    "build_date" : "2019-01-24T11:27:09.439740Z",
    "build_snapshot" : false,
    "lucene_version" : "7.6.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

### Create index

```
curl -X PUT http://localhost:9200/schools
{
    "acknowledged": true
}
```

### Create Mapping and Add data

ES will auto-create the mapping according to the data provided in request body, we will use its bulk functionality to add more than one JSON object in this index.

```json
curl -X POST \
  http://localhost:9200/schools/school/3 \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 7b708a68-2f60-488a-ab3c-2f052466064c' \
  -H 'cache-control: no-cache' \
  -d '{
   "name":"Crescent School", "description":"State Board Affiliation", "street":"Tonk Road", 
   "city":"Jaipur", "state":"RJ", "zip":"176114","location":[26.8535922, 75.7923988],
   "fees":2500, "tags":["Well equipped labs"], "rating":"4.5"
}'
```

Result: 

```json
{
    "_index": "schools",
    "_type": "school",
    "_id": "3",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

Stats: 

```
curl -XGET 'localhost:9200/_cat/indices?v&pretty'
health status index   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   schools zzgi7cK2RDeA9wlu6NK0uA   5   1          3            0     25.5kb         25.5kb
```

### API Conventions

**Comma Separated Notation**

`POST http://localhost:9200/index1,index2,index3/_search` 

```json
{
   "query":{
      "query_string":{
         "query":"any_string"
      }
   }
}
```

=> Gives JSON objects from index1, index2, index3 having any_string in it.

**_all keyword for all indices**

`POST http://localhost:9200/_all/_search`

=> Gives JSON objects from all indices and having any_string in it.

**Wildcards ( * , + , – )**

`POST http://localhost:9200/school*/_search`

```
{
   "query":{
      "query_string":{
         "query":"CBSE"
      }
   }
}
```

=> Gives JSON objects from all indices which start with school having CBSE in it.

`POST http://localhost:9200/school*,-schools_gov /_search`

=> Gives JSON objects from all indices which start with “school” but not from schools_gov and having CBSE in it.

**Ignore unavailable**

`POST http://localhost:9200/school*,book_shops/_search?ignore_unavailable = true`

=> No 500 error and gives JSON objects from all indices which start with school having CBSE in it. 

**Allow no indices**

`POST http://localhost:9200/schools_pri*/_search?allow_no_indices=true`

=> will prevent error, if a URL with wildcard results in no indices.

```
{
   "took":1,"timed_out": false, "_shards":{"total":0, "successful":0, "failed":0}, 
   "hits":{"total":0, "max_score":0.0, "hits":[]}
}
```

**expand_wildcards**

`POST http://localhost:9200/schools/_close` => `{"acknowledged":true}`

`POST http://localhost:9200/school*/_search?expand_wildcards=closed` 

```json
{
   "error":{
       ...
   }
}
```

**Pretty results** 

`POST http://localhost:9200/schools/_search?pretty=true`

### Document APIs

**Automatic Index Creation**

When a request is made to add JSON object to a particular index and if that index does not exist then this API automatically creates that index and also the underlying mapping for that particular JSON object. This functionality can be disabled by changing the values of following parameters to false, which are present in elasticsearch.yml file.

```
action.auto_create_index:false
index.mapper.dynamic:false
```

**Versioning**

Elasticsearch also provides version control facility.

`POST http://localhost:9200/schools/school/1?version=1`

```json
{
   "name":"Central School", "description":"CBSE Affiliation", "street":"Nagan",
   "city":"paprola", "state":"HP", "zip":"176115", "location":[31.8955385, 76.8380405],
   "fees":2200, "tags":["Senior Secondary", "beautiful campus"], "rating":"3.3"
}
```

```json
{
    "_index": "schools",
    "_type": "school",
    "_id": "1",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 1,
    "_primary_term": 1
}
```

**Operation Type**

`POST http://localhost:9200/tutorials/chapter/1?op_type=create` to avoid overwrite.

**Automatic ID generation**
When ID is not specified in index operation, then Elasticsearch automatically generates id for that document.

**Parents and Children**
You can define the parent of any document by passing the id of parent document in parent URL query parameter.

`POST http://localhost:9200/tutorials/article/1?parent=1`

**Timeout**
By default, the index operation will wait on the primary shard to become available for up to 1 minute before failing and responding with an error. This timeout value can be changed explicitly by passing a value to timeout parameter.

`POST http://localhost:9200/tutorials/chapter/2?timeout=3m`

**Get API**

`GET http://localhost:9200/schools/school/1`

`GET http://localhost:9200/schools/school/1?fields=name,fees` 

**Delete API**

`DELETE http://localhost:9200/schools/school/4` 

**Update API**

`POST http://localhost:9200/schools_gov/school/1/_update` 

**Multi Get API**

`POST http://localhost:9200/_mget`

```json
{
   "docs":[
      {
         "_index": "schools", "_type": "school", "_id": "1"
      },
		
      {
         "_index":"schools_gev", "_type":"school", "_id": "2"
      }
   ]
}
```

### Search API

**Multi-index**

`GET http://localhost:9200/_search?q=name:central`

`GET http://localhost:9200/schools,schools_gov/_search?q=name:model`

**Multi-Type**

`Get http://localhost:9200/schools/_search?q=tags:sports` tags is an array.

**URI Search**

