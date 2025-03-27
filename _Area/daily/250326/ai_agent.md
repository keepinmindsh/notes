# ElasticSearch 

- 8.13 기반 구성하기 

```shell
docker run -d --name elasticsearch \
  -e "discovery.type=single-node" \
  -e "ELASTIC_PASSWORD=elastic" \
  -e "xpack.security.enabled=true" \
  -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
  -p 9200:9200 -p 9300:9300 \
  docker.elastic.co/elasticsearch/elasticsearch:8.13.0
```

# Kibana

```shell 
docker run -d \
  --name kibana \
  --network=host \
  -e ELASTICSEARCH_HOSTS=http://localhost:9200 \
  docker.elastic.co/kibana/kibana:8.13.0
```

## References 

- https://s-core.co.kr/insight/view/elastic-vectordb-%EA%B3%A0%EC%84%B1%EB%8A%A5-%EA%B2%80%EC%83%89%EC%9D%98-%EB%AF%B8%EB%9E%98/


> LangChain은 Elasticsearch의 KNN[^1] 검색을 시도한다. Elasticsearch버전이 7은 dense_vector검색시 ANN[^2] 검색을 시도한다. 
> 즉, LangChain을 사용하기 위해서는 버전이 8이상인 Elasticsearch가 필요하다!