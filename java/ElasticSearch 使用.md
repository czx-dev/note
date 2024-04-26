# ElasticSearch 使用

/_cat查看使用

put 请求或者post 保存数据 

 / _update 会对比数据  数据没变化贼不叠加 版本 不加 _update,只会迭代版本号 

 ?_seq_no=1&_primary_term=1  乐观锁修改需要加上这个查询参数


 get 请求  查询数据  

delete 删除数据

批量操作  /bluk

/_search 查询语句

"query  "{}查询

​		"match":{} 分词查询   全文检索 然后打分

​		"match_all":查询所有

​		“match_phrase”:{}  短语匹配 不分词

​		match

​		"sort":排序 

​		"from":1,"size":1 相当于 limt 起始 ,大小

"term":  {} 用于非文本字段查询

```
//查询 字段address| city 中 包含 mail | movico 的文档
GET 索引/_search
{
	"query"{
	"multi_match":{
	"query":"mall movico",
	"fields":["address", "city"]
		}
	}
}
```

```
//查询 字段address| city 中 包含 mail | movico 的文档
GET 索引/_search
{
	"query"{
		"bool":{
		"must":[{"match":{}},{...}]//必须满足
		"must_not":[{},{}]//必须不满足
		"should":[{},{}] //满足加分  不满足也可以
		}	
	}
}
```

“filter”:{}

“aggs”:{"别名":{聚合操作}

​		,:"aggs":{同上}//输入为上步操作结果

}

“mapping”:{"

​	"properties":{

 字段: 类型}	

"}

