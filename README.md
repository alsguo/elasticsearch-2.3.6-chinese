# An optimized Chinese version of Elastic Search


##优化的部分有
*增加了词库生物化学方面的词库
*改变默认设置，让其支持groov表达式以支持质量分加权排序
##增加词库

优化的部分在elasticsearch-2.3.5/plugins下面
```
└── ik
    ├── commons-codec-1.9.jar
    ├── commons-logging-1.2.jar
    ├── config
    │   ├── bie-glossary.dic
    │   ├── custom
    │   │   ├── ext_stopword.dic
    │   │   ├── mydict.dic(*****)
    │   │   ├── single_word.dic
    │   │   ├── single_word_full.dic
    │   │   ├── single_word_low_freq.dic
    │   │   └── sougou.dic
    │   ├── IKAnalyzer.cfg.xml
    │   ├── main.dic
    │   ├── preposition.dic
    │   ├── quantifier.dic
    │   ├── stopword.dic
    │   ├── suffix.dic
    │   ├── surname.dic
    │   └── test.txt
    ├── elasticsearch-analysis-ik-1.9.5.jar
    ├── elasticsearch-analysis-ik-1.9.5.zip
    ├── httpclient-4.5.2.jar
    ├── httpcore-4.4.4.jar
    └── plugin-descriptor.properties

```


##重建索引
```
curl -XDELETE http://localhost:9200/index
curl -XPUT http://localhost:9200/index

//This is just an example

curl -XPOST http://localhost:9200/index/fulltext/_mapping -d'
{
    "fulltext": {
             "_all": {
            "analyzer": "ik_smart",
            "search_analyzer": "ik_smart",
            "term_vector": "no",
            "store": "false"
        },
    "properties": {
            "content": {
                "type": "string",
                "analyzer": "ik_smart",
                "search_analyzer": "ik_smart",
                "include_in_all": "true",
                "boost": 8
            },
            "rank" : {"type" : "float","boost":1}

        }
    }
}'
```


##修改查询
```

curl -XPOST http://localhost:9200/index/fulltext/_search?pretty   -d'
{
  "query": {

    "function_score": {
      "functions": [
        {
          "script_score": {
          "lang":"groovy",
            "script": "_score * (1+doc['"'"'rank'"'"'].value*0.1)"
          }
        }
      ],
      "query": {
        "match": {
            "content": "裂解液试剂盒使用"
        }}
    }
  }
}'

```
