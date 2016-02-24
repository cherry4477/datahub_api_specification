###同步服务
	将统计所需数据从kafka中同步到mysql中
	每天的数据存储到日表中，如果出现历史数据，删除历史信息，再插入统计数据
    如果数据量比较大，可分多批传输，每批最多100条
	topic 名称设置为：statistics
	kafka消息格式参考如下：
    数据说明
		service:服务名
        date：统计的日期（yyyy-MM-dd）
		table:表名
		columns：字段名称定义，与后面result里面的名字对应
			column_name：字段名称（注意，字段必须包括ID 序号作为主键）
   			column_type：字段类型 目前支持（String,int,long,time(传值时，给long型的毫秒数)）
   			column_length:长度（String类型的需要加）
		result：统计结果
    Example
		{
			"service":"user",
			"date":"2015-01-02",
			"table":"user",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"login_name",
					"column_type":"string",
					"column_length":32
				},	
				{
					"column_name":"user_type",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":"1",
					"login_name":"aa@126.com",
					"user_type":3
				},	
				{
					"id":"2",					
					"login_name":"bb@126.com",
					"user_type":4}
				]
		}

###user服务
	数据说明
		login_name：用户登录名称
		user_type：用户级别(1：普通用户，2：管理员用户，3:认证会员,4：金卡会员，5：钻石会员)
	Example
		{
			"server":"user",
			"date":"2015-01-02",
			"table":"user",
			"columns":[
				{
					"column_name":"login_name",
					"column_type":"string",
					"column_length":32
				},	
				{
					"column_name":"user_type",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":1,
					"login_name":"aa@126.com",
					"user_type":3
				},	
				{
					"id":2,
					"login_name":"bb@126.com",
					"user_type":4}
				]
		}

	
###star服务
	数据说明
		REPOSITORY_NAME string(64) 
		DATAITEM_NAME string(64)
		NUM_STARS int 总订购数
	Example
		{
			"service":"subscriptions",
			"date":"2015-01-02",
			"table":"dataitem_numstars",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"REPOSITORY_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"DATAITEM_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"NUM_STARS",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":1,
					"REPOSITORY_NAME":"repo01",
					"DATAITEM_NAME":"item02",
					"NUM_STARS":3
				},	
				{
					"id":2,
					"REPOSITORY_NAME":"repo07",
					"DATAITEM_NAME":"item07",
					"NUM_STARS":4
				]
		}

###subscription服务
	数据说明
		REPOSITORY_NAME string(64) 
		DATAITEM_NAME string(64)
		NUM_SUBS int 总订购数
	Example
		{
			"service":"subscriptions",
			"date":"2015-01-02",
			"table":"dataitem_numsubs",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"REPOSITORY_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"DATAITEM_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"NUM_SUBS",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":1,
					"REPOSITORY_NAME":"repo01",
					"DATAITEM_NAME":"item02",
					"NUM_SUBS":3
				},	
				{
					"id":2,
					"REPOSITORY_NAME":"repo07",
					"DATAITEM_NAME":"item07",
					"NUM_SUBS":4}
				]
		}

	
###transaction服务
	数据说明
		REPOSITORY_NAME string(64) 
		DATAITEM_NAME string(64)
		TAG string(64)
		NUM_PULLS int 总下载数
	Example
		{
			"service":"transactions",
			"date":"2015-01-02",
			"table":"dataitem_numpulls",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"REPOSITORY_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"DATAITEM_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"NUM_PULLS",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":1,
					"REPOSITORY_NAME":"repo01",
					"DATAITEM_NAME":"item02",
					"NUM_PULLS":3
				},	
				{
					"id":2,
					"REPOSITORY_NAME":"repo07",
					"DATAITEM_NAME":"item07",
					"NUM_PULLS":4
				}
				]
		}

###comment服务
	数据说明
		REPOSITORY_NAME string(64) 
		DATAITEM_NAME string(64)
		NUM_COMMENTS int 总订购数
	Example
		{
			"service":"subscriptions",
			"date":"2015-01-02",
			"table":"dataitem_numsubs",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"REPOSITORY_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"DATAITEM_NAME",
					"column_type":"string",
					"column_length":64
				},	
				{
					"column_name":"NUM_COMMENTS",
					"column_type":"int"
				}
			], 
		   "result":[
				{
					"id":1,
					"REPOSITORY_NAME":"repo01",
					"DATAITEM_NAME":"item02",
					"NUM_COMMENTS":3
				},	
				{
					"id":2,
					"REPOSITORY_NAME":"repo07",
					"DATAITEM_NAME":"item07",
					"NUM_COMMENTS":4
				},
			]
		}


###heartbeat服务
	数据说明
		id 			int 
		user 		string(128)
		daemonid 	string(48)
		entrypoint	string(128)		"", "http://54.223.72.171:35800"
		status		string(16) 		"online","offline"
		role		int 			0:puller, 1:publisher
		stattime	time 			millisecond
	Example
		{
			"service":"datahub_daemon",
			"date":"2015-01-02",
			"table":"datahub_daemon",
			"columns":[
				{
					"column_name":"id",
					"column_type":"int"
				},	
				{
					"column_name":"user",
					"column_type":"string",
					"column_length":128
				},	
				{
					"column_name":"daemonid",
					"column_type":"string",
					"column_length":48
				},	
				{
					"column_name":"entrypoint",
					"column_type":"string"
					"column_length":128
				},
				{
					"column_name":"status",
					"column_type":"string",
					"column_length":16
				},
				{
					"column_name":"role",
					"column_type":"int",
				},
				{
					"column_name":"stattime",
					"column_type":"time",
				},
			], 
		   "result":[
				{
					"id":150000001,
					"user":"zhangwy@asiainfo.com",
					"daemonid":"cf71bce910c7436f04f72959abcfe8ff652d4f11",
					"entrypoint":"",
					"status":"offline",
					"role":0,
					"stattime":1453100439000
				},	
				{
					"id":150000002,
					"user":"yaxin@asiainfo.com",
					"daemonid":"d52c96a2e426728198d13c93d93098208adcb602",
					"entrypoint":"http://54.223.72.171:35850",
					"status":"online",
					"role":1,
					"stattime":1453100439000
				},
			]
		}


###repositor服务
    数据说明    
    	id                  数据序列号
    	repuser             rep创建者
    	repname             rep名称
    	repaccesstype       rep访问权限
    	itemuser            item创建者
    	itemname            item名称
    	tags                item下tag数量
    	itemaccesstype      item访问权限
    	supplestyle         item服务形式
    	selectlabel         item精选名称
    	
    Example	
    	{
        	"service":"repository",
        	"date":"2015-01-02",
        	"columns":[
        		        {
        			        "column_name":"id",
        			        "column_type":"int",
        		        },	
        		        {
                            "column_name":"repuser",
                            "column_type":"string",
                        },
                        {
                            "column_name":"repname",
                            "column_type":"string",
                        },        		
        		        {
                            "column_name":"repaccesstype",
                            "column_type":"string",
                        },
        		        {
                            "column_name":"itemuser",
                            "column_type":"string",
                        },
        		        {
                            "column_name":"itemname",
                            "column_type":"string",
                        },
        		        {
                            "column_name":"tags",
                            "column_type":"int",
                        },
        		        {
                            "column_name":"itemaccesstype",
                            "column_type":"string",
                        },
                        {
                            "column_name":"supplestyle",
                            "column_type":"string",
                        },
                        {
                            "column_name":"itemaccesstype",
                            "column_type":"selectlabel",
                        }                        		        	
        	        ], 
        	"result":[
        			    {
        					"id": 0,             
                            "repuser": "panxy3@asiainfo.com",
                            "repname": "test", 
                            "repaccesstype" : "public",
                            "itemuser": "liuxu@asiainfo.com",            
                            "itemname": "test2",            
                            "tags" : "12",               
                            "itemaccesstype" : "private",     
                            "supplestyle": "batch",         
                            "selectlabel" : "征信专题",        
        			    },	
        	]
        }
	
## topic: to_repositories.json

### 增加repository rank
    
    key: add_statis_rank_rep
	
	{
		"repname": "repo001",
        "rank": 10.5,
        "time": "2016-1-24 23-12-12"		
	}
	注意:rank不是字符串

### 增加dataitem rank
    
    key: add_statis_rank_rep
	{
		"repname": "repo001",
		"itemname": "item002",
        "rank": 10.2,
        "time": "2016-1-24 23-12-12"		
	}	
	注意:rank不是字符串
        
