# ES-Tutorial-1

ElasticSearch 첫번째 튜토리얼을 기술합니다.

## Product 별 버전 상세
```
Product Version. 6.6.0(2019/02/07 기준 Latest Ver.)
```
* [Elasticsearch](https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.6.0.rpm)
* [Kibana](https://artifacts.elastic.co/downloads/kibana/kibana-6.6.0-x86_64.rpm)

최신 버전은 [Elasticsearch 공식 홈페이지](https://www.elastic.co/downloads) 에서 다운로드 가능합니다.

## ElasticSearch Product 설치

이 튜토리얼에서는 rpm 파일을 이용하여 실습합니다.

```bash
[ec2-user@ip-xxx-xxx-xxx-xxx ~]$ sudo yum -y install git

[ec2-user@ip-xxx-xxx-xxx-xxx ~]$ git clone https://github.com/benjamin-btn/ES-Tutorial-1.git

[ec2-user@ip-xxx-xxx-xxx-xxx ~]$ cd ES-Tutorial-1

[ec2-user@ip-xxx-xxx-xxx-xxx ES-Tutorial-1]$ ./tuto1
##################### Menu ##############
 $ ./tuto1 [Command]
#####################%%%%%%##############
         1 : install java & elasticsearch packages
         2 : configure elasticsearch.yml & jvm.options
         3 : start elasticsearch process
         4 : install kibana packages
         5 : configure kibana.yml
         6 : start kibana process
#########################################


[ec2-user@ip-xxx-xxx-xxx-xxx ES-Tutorial-1]$ ./tuto1 1

```

## ELK Tutorial 1 - Elasticsearch, Kibana 세팅

### Elasticsearch
* packages/elasticsearch/config/elasticsearch.yml
  - network.host, http.cors.enabled, http.cors.allow-origin 추가설정

* packages/elasticsearch/config/jvm.options
  - Xms1g, Xmx1g 를 물리 메모리의 절반으로 수정

```bash
$ vi packages/elasticsearch/config/elasticsearch.yml
+ network.host: 0.0.0.0
+ http.cors.enabled: true
+ http.cors.allow-origin: "*"

$ vi packages/elasticsearch/config/jvm.options

- -Xms1g
+ -Xms4g
- -Xmx1g
+ -Xmx4g
```

### Kibana
* packages/kibana/config/kibana.yml
  - server.host: "0.0.0.0" -> 외부에서 접근 가능하도록 변경
  - elasticsearch.url: "http://localhost:9200" -> 주석해제
  - kibana.index: ".kibana" -> 주석해제

```bash
$ vi packages/kibana/config/kibana.yml
- #server.host: "localhost"
+ server.host: "0.0.0.0"
- #elasticsearch.url: "http://localhost:9200"
+ elasticsearch.url: "http://localhost:9200"
- #kibana.index: ".kibana"
+ kibana.index: ".kibana"
```

## Smoke Test

### Elasticsearch

```bash
$ curl localhost:9200

{
  "name" : "UZU8SQG",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "YP3Lt-53QP6H5Ay_M6UTjw",
  "version" : {
    "number" : "6.5.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "9434bed",
    "build_date" : "2018-11-29T23:58:20.891072Z",
    "build_snapshot" : false,
    "lucene_version" : "7.5.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

$ curl -H 'Content-Type: application/json' -XPOST localhost:9200/firstindex/_doc -d '{ "mykey": "myvalue" }'
```

### Kibana
* Web Browser 에 http://{FQDN}:5601 실행
