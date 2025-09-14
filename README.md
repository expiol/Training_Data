* * *

# 固定关系与数据来源

## 十二个固定的关系

* admired professor is
    
* favorite attraction is
    
* favorite book is
    
* favorite celebrity is
    
* favorite food is
    
* favorite movie is
    
* favorite music piece is
    
* favorite sports brand is
    
* lives in
    
* studies at
    
* trusts the medical brand
    
* works for
    

* * *

## 元数据来源
**网址**：[https://dbpedia.org/sparql](https://dbpedia.org/sparql)


* **University.json**：来源于 **QS 2025 排行榜**
    

* * *

## 随机城市生成

从 DBpedia 获取随机城市：

```sparql
SELECT ?city ?name
WHERE {
  ?city rdf:type dbo:City .
  ?city rdfs:label ?name .
  FILTER (lang(?name) = 'en')
}
ORDER BY RAND()
LIMIT 1
```

* `rdf:type dbo:City` → 只获取城市实体
    
* `rdfs:label ?name` → 获取城市名称
    
* `FILTER (lang(?name) = 'en')` → 只取英文名称
    
* `ORDER BY RAND()` → 随机排序
    
* `LIMIT 1` → 只取一个随机城市
    

* * *

## 体育品牌查询（Sports Brands）

```sparql
SELECT ?brand ?name ?abstract
WHERE {
  ?brand rdf:type dbo:Company .                       ## 只要类型是公司 (dbo:Company) 的实体
  ?brand rdfs:label ?name .
  ?brand dbo:abstract ?abstract .                     ## 获取公司名字和简介
  FILTER (lang(?name) = 'en' && lang(?abstract) = 'en')  ## 确保只取英文名称和英文简介
  FILTER (
    CONTAINS(LCASE(?abstract), "sports") || 
    CONTAINS(LCASE(?abstract), "athletic") || 
    CONTAINS(LCASE(?abstract), "footwear") || 
    CONTAINS(LCASE(?abstract), "badminton") || 
    CONTAINS(LCASE(?abstract), "tennis") || 
    CONTAINS(LCASE(?abstract), "equipment")
  )                                                  ## 只要简介中包含任意一个关键词
}
LIMIT 200
```

* 用于查找 **运动相关公司**
    
* 关键词匹配公司简介 (`abstract`)
    

* * *

## 教授查询（Professors）

```sparql
SELECT ?person ?name ?abstract 
WHERE {
  ?person dbo:occupation dbr:Professor .            ## 只返回被明确标注职业是教授的人
  ?person rdfs:label ?name .
  ?person dbo:abstract ?abstract .
  FILTER (lang(?name) = 'en' && lang(?abstract) = 'en')
}
LIMIT 500
```

* 查询 **教授职业的人物**
    
* 返回名字和简介
    
* 注意：严格匹配 `dbr:Professor`，可能遗漏部分教授
    

* * *

## 书籍查询（Books）

```sparql
SELECT ?book ?name ?abstract
WHERE {
  ?book rdf:type dbo:Book .                         ## 只返回标注为书的实体
  ?book rdfs:label ?name .
  ?book dbo:abstract ?abstract .
  FILTER (lang(?name) = 'en' && lang(?abstract) = 'en')
}
LIMIT 200
```

* 查询 **书籍实体**
    
* 返回名字和简介
    
* 可根据需要添加关键词过滤
    

* * *
