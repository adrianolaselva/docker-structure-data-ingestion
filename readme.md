
##Ambiente elasticsearch docker

*   Elasticsearch 6.2.1
*   Kibana 6.2.1


###Exemplos de operações mais utilizadas com elasticsearch



####Criar novo document type

```ssh
curl -X PUT \
  http://elastic:changeme@localhost:9200/report \
  -H 'Content-Type: application/json' \
  -d '{
    "settings" : {
        "index" : {
            "number_of_shards" : 3,
            "number_of_replicas" : 2
        }
    }
}'
```

####remover document type
```ssh
curl -X DELETE \
  http://elastic:changeme@localhost:9200/report \
  -H 'Content-Type: application/json'
```

####Inserir registro
```ssh
curl -X POST \
  http://elastic:changeme@localhost:9200/report/data_01 \
  -H 'Content-Type: application/json' \
  -d '{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch",
    "value": 10.99
}'
```

####Alterar registro
```ssh
curl -X PUT \
  http://elastic:changeme@localhost:9200/report/data_01/3IsRDGIB3KaJWBoIZ7ah \
  -H 'Content-Type: application/json' \
  -d '{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elasticsearch",
    "value": 10.99
}'
```

####Carregar registro
```ssh
curl -X GET \
  http://elastic:changeme@localhost:9200/report/data_01/_search \
  -H 'Content-Type: application/json'
  -d '{
  "query": {
    "query_string": {
      "query": "post_date: [2018-01-30T TO 2018-02-05T] and !user: kimch*"
    }
  },
  "sort": [
    {
      "data": {
        "order": "desc"
      }
    }
  ]
}'
```

####Remover registro
```ssh
curl -X DELETE \
  http://elastic:changeme@localhost:9200/report/data_01/3IsRDGIB3KaJWBoIZ7ah \
  -H 'Content-Type: application/json'
```

####Remover por query
```ssh
curl -X POST \
  http://elastic:changeme@localhost:9200/report/data_01/_delete_by_query?conflicts=proceed \
  -H 'Content-Type: application/json' \
  -d '{
  "query": {
    "query_string": {
      "query": "user: kimchy"
    }
  }
}'
```

####Alterar por query
```ssh
curl -X POST \
  http://elastic:changeme@localhost:9200/report/data_01/_update_by_query?conflicts=proceed \
  -H 'Content-Type: application/json' \
  -d '{
   "script":{
      "inline":"ctx._source.user='kimchy-alter';ctx._source.geoLocation=['lat': -8.1175125,'lon':-34.8996389]",
      "lang":"painless"
   },
   "query": {
     "query_string": {
       "query": "user: kimchy"
     }
   }
}'
```

####Carrega registros agrupados por data especificando o formato
```ssh
curl -X GET \
  http://elastic:changeme@localhost:9200/report/data_01/_search \
  -H 'Content-Type: application/json' \
  -d '{
  "size": 0, 
  "aggs": {
    "post_date": {
      "date_histogram": {
        "field": "post_date",
        "interval" : "day",
        "format" : "yyyy-MM-dd"
      }, 
      "aggs": {
        "value": {
          "sum": {
            "field": "value"
          }
        }
      }
    }
  }, 
  "query": {
    "query_string": {
      "query": "data: [2000-01-01 TO 2019-03-31]"
    }
  }
}'
```

####Reindexiar
```ssh
curl -X POST \
  http://elastic:changeme@localhost:9200/report/_reindex \
  -H 'Content-Type: application/json' \
  -d '{
  "source": {
    "index": "data_01"
  },
  "dest": {
    "index": "data_01_new"
  },
  "script": {
    "source": "if(ctx._source.value == 10.99 ) {ctx._source.value = 11.0}",
    "lang": "painless"
  }
}'
```

####Visualizar mapeamento de index
```ssh
curl -X GET \
  http://elastic:changeme@localhost:9200/report/data_01/_mapping \
  -H 'Content-Type: application/json'
```

####Alterar mapeamento de index
```ssh
curl -X PUT \
  http://elastic:changeme@localhost:9200/report/_mapping/data_01 \
  -H 'Content-Type: application/json' \
  -d '{
    "properties": {
        "message": {
            "type": "text",
            "fields": {
                "keyword": {
                    "type": "keyword",
                    "ignore_above": 256
                }
            }
        },
        "post_date": {
            "type": "date"
        },
        "user": {
            "type": "text",
            "fields": {
                "keyword": {
                    "type": "keyword",
                    "ignore_above": 256
                }
            }
        }
    }
}'
```



