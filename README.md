
# Ingestão de dados postgres para elasticsearch


## Ambiente elasticsearch docker oara 

*   Elasticsearch 6.2.1
*   Kibana 6.2.1
*   Logstash 6.2.1
*   Postgres 11.2

#### Conexão postgres

```sh
psql -h localhost -U conciliator -p 15432
```

```sh
aws sqs send-message --queue-url=https://sqs.us-east-1.amazonaws.com/420945218315/queue_transactions \
--message-body='{"id": "9999", "usuario": "9999", "adquirente": "CIELO", "bandeira": "MASTERCARD", "operacao": "VENDA_DEBITO", "afiliacao": "9999", "data" : "2019-01-01T00:00:00.000Z"}'
```






