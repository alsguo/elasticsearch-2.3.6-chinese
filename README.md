# elasticsearch-2.3.6-chinese
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
