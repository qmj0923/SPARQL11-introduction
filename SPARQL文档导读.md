# SPARQL文档导读

本文已在Github开源，可前往查看：https://github.com/qmj0923/SPARQL11-introduction

## 基础部分

SPARQL是一种用于查询RDF数据的编程语言。Wikidata是一个存储RDF数据的知识库，因此可以编写SPARQL代码来查询Wikidata数据。

本文档旨在帮助大家更高效地入门“使用SPARQL查询Wikidata”。需要阅读的教程有两份，分别是Wikidata的SPARQL教程（可细读）和SPARQL1.1官方文档（略读）。在初学阶段，只需掌握教程中的部分内容即可。其余内容可以等到要用时再学。

```
# Wikidata的SPARQL教程
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial
# SPARQL1.1官方文档
https://www.w3.org/TR/2013/REC-sparql11-query-20130321/
```

### 三元组

如果已经能够理解下面的SPARQL查询，那么可以速读（或跳过）本小节。

```
SELECT ?child WHERE {
# ?child  father   Bach
  ?child wdt:P22 wd:Q1339.
}
```

本小节需要学习的内容是Wikidata的SPARQL教程。需要从头开始读，读到下面的链接为止。其中出现的“SERVICE wikibase:label ...”相关的内容请直接跳过。

```
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Autocompletion
```

速读可以直接看中文版。

```
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial/zh
```

### 题目1

查询Johann Christoph Friedrich Bach（Q57225）的母亲是谁。

### 题目2

用自然语言描述一下，下面的查询想要查的问题是什么。

```
select ?obj where { wd:Q188920 wdt:P2813 ?obj . ?obj wdt:P31 wd:Q1002697 } 
```

### 谓词-宾语列表

本小节需要学习的内容：

```
# 从这个链接开始
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Advanced_triple_patterns
# 读到这个链接为止
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Instances_and_classes

# SPARQL1.1官方文档中的相应部分（4.1.4节-4.2.2节）（可暂时不读）
https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#QSynBlankNodes
```

需要掌握的内容：

```
SELECT ?child WHERE {
  ?child wdt:P22 wd:Q1339;
         wdt:P25 wd:Q57487;
         wdt:P106 wd:Q36834, wd:Q486748.
}
```

### 题目3

把上面的查询改成“只用最基本的三元组”的版本（不出现分号和逗号）。

### 属性路径

如果对正则表达式不是很熟悉的话，这部分内容可以**先跳过不学**。只需要（死记硬背地）记下：出现在三元组谓词位置上的“wdt:P31/wdt:P279*”表示“属于某种类型”即可。

```
# 从这个链接开始
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Instances_and_classes
# 读到这个链接为止
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Qualifiers

# SPARQL1.1官方文档中的相应部分（第9节）
https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#propertypaths
```

### 限定符

这一节很重要！而且相对比较难学。请仔细阅读（结合实体的Wikidata页面，可以看得清晰一点）。

```
# 从这个链接开始
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Qualifiers
# 读到这个链接为止
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#ORDER_and_LIMIT

# 回顾一下“空白节点”相关的内容
SPARQL1.1官方文档4.1.4节
https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#QSynBlankNodes
```

需要掌握的内容：

1. 三元组和statement节点

```
# 下面的查询是等价的
SELECT ?child WHERE { ?child wdt:P22 wd:Q1339. }
SELECT ?child WHERE { ?child p:P22 ?st. ?st ps:P22 wd:Q1339. }
SELECT ?child WHERE { ?child p:P22 [ ps:P22 wd:Q1339 ]. }
```

2. qualifier的使用

```
SELECT ?material WHERE {
  wd:Q12418 p:P186 [ ps:P186 ?material; pq:P518 wd:Q861259 ].
}
```

### 题目4

查询奥巴马（Q76）就读哈佛（Q13371）的入学时间。

### 过滤器

本小节需要学习的内容：

```
# 从这个链接开始
https://www.wikidata.org/wiki/Wikidata:SPARQL_tutorial#Qualifiers
# 读到下面这句话为止
FILTER(YEAR(?dob) = 2015).

# SPARQL1.1官方文档中的相应部分（第3节）（推荐读一下，比较适合初学者）
https://www.w3.org/TR/2013/REC-sparql11-query-20130321/#termConstraint
```

需要掌握的内容：

```
SELECT ?person ?dob WHERE {
  ?person wdt:P31 wd:Q5;
          wdt:P569 ?dob.
  FILTER(YEAR(?dob) = 2015).
}
```

### 题目5

用自然语言描述一下，下面的查询想要查的问题是什么。

```
SELECT ?result WHERE {
    wd:Q69303989 wdt:P57 ?dir. 
    ?result wdt:P31 wd:Q11424; 
            wdt:P57 ?dir; 
            wdt:P161 wd:Q294647;
    FILTER(?result != wd:Q69303989)
}
```

### 题目6

填写完成下面的查询。

```
# 需要查询的问题：
# What other civilizations existed during the Aztecs ?
# 在阿茲特克古文明（Q12542）存在的时期内，还有哪些其他文明？

SELECT ?result WHERE {
    ?result wdt:P31/wdt:P279* wd:Q8432. # ?result是一个文明
    # TODO: 填写剩余内容
}
```



## 下一步

### 入门后的学习内容

学到这里已经差不多入门了。下一步的学习可以把Wikidata的SPARQL教程的剩余部分通读一下。在日后实践过程中，会相对频繁地遇到ORDER BY、GROUP BY、COUNT等SPARQL的关键字。这些内容是本文档不曾涉及的，需要大家自行阅读官方教程来学习。




## 参考答案

```
# 题目1
SELECT ?mother WHERE {
  wd:Q57225 wdt:P25 ?mother.
}

# 题目2
What periodical literature does Delta Air Lines use as a moutpiece?
问句来源：LCQuAD2.0训练集(uid-19719)

# 题目3
SELECT ?child WHERE {
  ?child wdt:P22 wd:Q1339.
  ?child wdt:P25 wd:Q57487.
  ?child wdt:P106 wd:Q36834.
  ?child wdt:P106 wd:Q486748.
}

# 题目4
SELECT ?t WHERE {
  wd:Q76 p:P69 [ ps:P69 wd:Q13371; pq:P580 ?t].
}

# 题目5
Which other movies by the director of Another Round also starred Mads Mikkelsen ?
问句来源：QALD10

# 题目6
SELECT ?result WHERE {
    ?result wdt:P31/wdt:P279* wd:Q8432. 
    ?result wdt:P580 ?start;
        wdt:P582 ?end. 
    wd:Q12542 wdt:P580 ?ast; 
        wdt:P582 ?aet. 
    FILTER(?start < ?aet)
    FILTER(?end > ?ast)
    FILTER(?result != wd:Q12542)
}
注：FILTER试图构造的约束是“?ast < ?start < ?end < ?aet”，不一定要按照上面那么写。
问句来源：QALD10
```


