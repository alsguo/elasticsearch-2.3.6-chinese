# elasticsearch-2.3.6-chinese

##优化的部分有
*增加了词库生物化学方面的词库
*改变默认设置，让其支持groov表达式以支持质量分加权排序
##增加词库

优化的部分在
```
└── ik
    ├── commons-codec-1.9.jar
    ├── commons-logging-1.2.jar
    ├── config
    │   ├── bie-glossary.dic
    │   ├── custom
    │   │   ├── bettbiotest.txt
    │   │   ├── ext_stopword.dic
    │   │   ├── mydict.dic
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


An optimized Chinese version of Elastic Search


curl -XDELETE http://localhost:9200/index
curl -XPUT http://localhost:9200/index


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
