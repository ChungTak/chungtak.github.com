---
layout: post
title: "hibernate多对多查询"
date: 2012-10-11 09:35
comments: true
categories: JAVA 
---

示例：每篇文章（article）与标签（tag）为多对多关系  
![img](http://ww2.sinaimg.cn/large/a74ecc4cjw1dxr0mgy0vdj.jpg "")  
Hibernate 映射: 
 
``` xml
<hibernate-mapping>
    <class name=com.sample.model.Tag" table="tag">
        <cache usage="read-write"/>
        <id name="id" column="id" type="long">
            <generator class="native"/>
        </id>
        <property name="name" column="name"/>
    </class>
</hibernate-mapping>
```

``` xml
<hibernate-mapping>
    <class name="ca.sergiy.model.Article" table="article">
        <cache usage="read-write" />
        <id name="id" column="id" type="long">
            <generator class="native" />
        </id>
        <property name="title" column="title" />
        <set name="tags" table="article_tag" lazy="false">
            <key column="articleid" />
            <many-to-many class="ca.sergiy.model.Tag" column="tagid" />
        </set>
    </class>
</hibernate-mapping>
```

#### 1. 根据标签（tag）查询文章（article）####
MYSQL查询语句：查询标签为 "Java" 或者 标签为"Hibernate" 的文章  

``` sql
SELECT DISTINCT a.*
FROM   `article` a
       INNER JOIN article_tag at
         ON at.articleid = a.id
       INNER JOIN tag t
         ON t.id = at.tagid
WHERE  t.name IN ("Java", "Hibernate")
```

Hibernate HQL:  
``` sql
String[] tags = {"Java", "Hibernate"};
String hql = "select distinct a from Article a " +
                "join a.tags t " +
                "where t.name in (:tags)";
Query query = session.createQuery(hql);
query.setParameterList("tags", tags);
List<Article> articles = query.list();
```
<!--more-->

#### 2. 查询没有标签的文章 ####
MySQL 查询语句:  
``` sql
SELECT   a.*
FROM     `article` a
         LEFT JOIN article_tag at
           ON at.articleid = a.id
GROUP BY a.id
HAVING   Count(at.tagid) = 0
```

Hibernate HQL:    
``` sql
String hql = "select a from Article a " +
                "left join a.tags t " +
                "group by a " +
                "having count(t)=0";
Query query = session.createQuery(hql);
List<Article> articles = query.list();
```  
注意这里使用了`LEFT JOIN`.

#### 3. 查询文章至少包含特定标签 ####
查询至少包含标签是"Java" 和标签 "Hibernate" 的文章(文章至少含有这2个标签)  
MySQL 查询语句:  
``` sql
SELECT a.*
FROM   article a
       INNER JOIN (SELECT   at.articleid
                   FROM     article_tag at
                            INNER JOIN article a
                              ON a.id = at.articleid
                            INNER JOIN tag t
                              ON t.id = at.tagid
                   WHERE    t.name IN ("Java","Hibernate")
                   GROUP BY at.articleid
                   HAVING   Count(at.articleid) = 2) aa
         ON a.id = aa.articleid
```

Hibernate HQL:    
``` sql
String[] tags = {"Java", "Hibernate"};
String hql = "select a from Article a " +
                "join a.tags t " +
                "where t.name in (:tags) " +
                "group by a " +
                "having count(t)=:tag_count";
Query query = session.createQuery(hql);
query.setParameterList("tags", tags);
query.setInteger("tag_count", tags.length);
List<Article> articles = query.list();
```

#### 4. 查询文章指定多个标签 ####
查询标签是"Java" 和标签 "Hibernate" 的文章(文章只指派了这2个标签)  
MySQL 查询语句:  
``` sql
SELECT a.*
FROM   article a
       INNER JOIN (SELECT   at.articleid
                   FROM     article_tag at
                   WHERE    at.articleid IN (SELECT   at2.articleid
                                             FROM     article_tag at2
                                                      INNER JOIN article a2
                                                        ON a2.id = at2.articleid
                                             GROUP BY at2.articleid
                                             HAVING   Count(at2.articleid) = 2)
                            AND at.tagid IN (SELECT id
                                             FROM   tag t
                                             WHERE  t.name IN ("Java","Hibernate"))
                   GROUP BY at.articleid
                   HAVING   Count(at.articleid) = 2) aa
         ON a.id = aa.articleid
```

Hibernate HQL:    
``` sql
String[] tags = {"Java", "Hibernate"};
String hql = "select a from Article a " +
                "join a.tags t " +
                "where t.name in (:tags) " +
                "and a.id in (" +
                    "select a2.id " +
                    "from Article a2 " +
                    "join a2.tags t2 " +
                    "group by a2 " +
                    "having count(t2)=:tag_count) " +
                "group by a " +
                "having count(t)=:tag_count";
Query query = session.createQuery(hql);
query.setParameterList("tags", tags);
query.setInteger("tag_count", tags.length);
List<Article> articles = query.list();
```




