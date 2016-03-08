# API 列表
	

- [GET] /repositories

- [GET] [POST] [DELETE] /repositories/:repname

- [GET] [POST] [DELETE] /repositories/:repname/:itemname

- [GET] [POST] [DELETE] /repositories/:repname/:itemname/:tag

- [GET] /repositories/:repname/:itemname/subpermission

- [GET] /repositories/deleted
	
----------

## 1 指令：GET /repositories

说明

	【拥有者】
	 		如果后面没有username的参数,则返回本人创建或具备写权限的所有repository,
	 		如果该repository名下有协作者,repository创建这返回repname及cooperate_status
	【任意】 
			如果带有参数?username=XXX，则表示查询XXX的所有repository，这里只能反馈查询者具备读权限的repositories名字（比如public或者在private白名单中的）

输入参数说明：
	
	page (分页页数) : 			1 - N，  默认=1（page=1可以不传）
	size（页面大小）: 			1 - N，  默认=6 (-1 返回全部)
	username: 					数据提供者username（header 中为登录用户的username）

Example Request：

	GET /repositories?page=5&size=8 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

返回数据说明：
	
	repname：数据仓库的名字
	cooperatestate: rep协作状态[协作中,协作]
	
返回值示例
        
	[
		{
			"repname": "mobile",
			"cooperatestate": "",
		}，
		{
			"repname": "mobile",
			"cooperatestate": "协作中",
		},
		{
			"repname": "mobile", 
			"cooperatestate": "协作",
		}
	]

----------

## 2 指令：GET /repositories/:repname
	
说明：
	
	【任意】查询repository详情

输入参数说明：

	items							查询repname的所有dataitem开关[1 ]
	relatedItems                    查询repname的相关dataitem开关[1 ]
	myRelease                       查询我的发布
	page (分页页数) : 				1 - N，  默认=1（page=1可以不传）
	size（页面大小）: 				1 - N，  默认=6 (-1 返回全部)
	
	
Example Request：

	GET /repositories/myrep1?items=1 HTTP/1.1 
	Accept: application/json

返回数据说明：
	
	create_user			创建者
   	repaccesstype       对外开放类型[public(默认), private]
    comment 			详情
	optime				更新时间
	items				dataitem数量	
	label				标签
	dataitems			dataitem的名称集	
	
返回值示例

	{
	    "create_user": "panxy3@asiainfo.com",
	    "repaccesstype": "public",
	    "comment": "详情",
	    "optime": "2015-10.1122: 10: 20",
	    "items": 3000,
	    "label": {},
	    "dataitems": [
	        "dataitem1",
	        "dataitem2"
	    ]
	}


## 3 指令：POST /repositories/:repname
	
说明：
	
	【任意】创建repository

输入参数说明：

   	repaccesstype       		访问权限[public(默认), private]
    comment 					详情
	label						label自定义json标签。
	label[sys,opt,owner,other]	如果为空可以不传
	label.sys					系统自定义label参数
	label.opt					运营人员定义label参数
	label.owner					拥有者自定义label参数
	label.other					管理员自定义label参数
    			
Example Request：

	POST /repositories/chinamobile HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	{
        "repaccesstype": "public",
        "comment": "中国移动北京终端详情",
        "label": {
            "sys": {
                "loc": "北京"
            },
            "opt": {
                "age": 22
            },
            "owner": {
                "name": "michael"
            },
            "other": {
                "friend": 22
            }
        }
    }

## 4 指令：PUT /repositories/:repname
	
说明：
	
	【拥有者】更新repository

输入参数说明：

   	repaccesstype       		访问权限[public(默认), private]
    comment 					详情
    			
Example Request：

	PUT /repositories/chinamobile HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	{
        "repaccesstype": "public",
        "comment": "中国移动北京终端详情",      
    }


## 5 指令：DELETE /repositories/:repname
	
说明：
	
	【拥有者】删除repository

输入参数说明：

	repname[必选]		
	
Example Request：

	DELETE /repositories/chinamobile HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

Example Response：

	无

## 6 指令：GET /repositories/:repname/:itemname
	
说明：
	
	【任意】返回dataitem详细信息

输入参数说明：

	page                                          taglist分页页数[1 - N，  默认=1（page=1可以不传）]
	size				                          taglist页面大小[1 - N，  默认=6]
	abstract		                              是否只显示摘要信息[1 只显示Item（optime，comment，label, tags, cooperate_status）5个属性]
	haspermission			                      返回是否具有订阅权限[1 显示是否具有订阅权限,不传则不显示订阅权限]

Example Request：

	GET /repositories/chinamobile/beijingphone HTTP/1.1 
	Accept: application/json

返回数据说明：
	
	create_user 			Dataitem创建者
	itemaccesstype  		访问权限[public(默认), private]
	optime   				更新时间
	meta					元数据
	sample					样例数据
	comment					详情
	price					计费计划
	price.units             购买数量
	price.money             价格
	price.expire            有效期(天)
	price.limit             限购次数
	price.plan_id           价格计划id
    tags					tag量
	label.sys.supply_style	服务形式[api；batch；flow]
	tags.tag				tag名称
	tags.comment			tag详情
	tags.optime				tag上传日期
	taglist					item下所有tag的详细信息
	permission              订阅权限[true, false]
	pricestate              价格状态[免费, 限量免费, 付费]
	cooperatestate          协作状态[协作,协作中]

返回值示例
        
	{
	    "create_user": "panxy3",
	    "itemaccesstype": "private",
	    "optime": "2015-08-0100: 00: 00",
	    "meta": {},
	    "sample": {},
		"price":[
					{
						"units": "30",
						"money": 5,
						"expire":30,
						"limit":1,
						"plan_id":"100000000000000000000000"
					},
					{
						"units": "300",
						"money": 50,
						"expire":30,
						"plan_id":"100000000000000000000000"
					},
					{
						"units: "3000",
						"money": 400,
						"expire":30,
						"plan_id":"100000000000000000000000"
					}
				],		
	    "comment": "对终端使用情况、变化情况进行了全方面的分析。包括分品牌统计市场存量、新增、机型、数量、换机等情况。终端与ARPU、DOU、网龄的映射关系。终端的APP安装情况等。",
		"tags":10
		"label": {
	        "sys": {
	            "supply_style": "flow"
	        },
	        "opt": {},
	        "owner": {},
	        "other": {}
	    },
	   "taglist":[
				{
					"tag": "tag001",
					"comment": "2022201MB",
					"optime": "2015-11-16 10:50:31.779460404 +0000 UTC|3小时以前"
				},
				{
					"tag": "tag002",
					"comment": "2001MB",
					"optime": "2015-11-17 02:14:55.241920929 +0000 UTC|10分钟以前"
				},
				{
					"tag": "tag003",
					"comment": "2001MB",
					"optime": "2015-11-17 02:14:59.491811069 +0000 UTC|6天以前"
				}
			]
	   "permission": true,
	   "pricestate": "免费",
	   "cooperatestate": "协作中",
	}

----------

## 7 指令：POST /repositories/:repname/:itemname

说明

	【拥有者】发布DataItem

输入参数说明
	
	itemaccesstype  				访问权限[public(默认), private]
	meta							元数据
	sample							样例数据
	comment							详情
	price					        计费计划
    price.units                     购买数量
    price.money                     价格
    price.expire                    有效期(天)
    price.limit                     限购次数【可选】
	label.sys.supply_style			服务形式[api；batch；flow]【必选】
	label.sys.supply_style.api		实时单条
	label.sys.supply_style.batch	批量
	label.sys.supply_style.flow		流式
			
Example Request：

	POST /repositories/chinamobile/beijingphone HTTP/1.1 
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	{
        "itemaccesstype": "private",
        "meta": "{}",
        "sample": "{}",
        "comment": "对终端使用情况、变化情况进行了全方面的分析。包括分品牌统计市场存量、新增、机型、数量、换机等情况。终端与ARPU、DOU、网龄的映射关系。终端的APP安装情况等。",
		"price":[
					{
						"units": "30",
                        "money": 5,
                        "expire":30,
                        "limit":1,
					},
					{
						"units": "30",
                        "money": 5,
                        "expire":30,
                        "limit":1,
					}
				],
        "label": {
            "sys": {
                "supply_style": "flow"
            },
            "opt": {},
            "owner": {},
            "other": {}
        }
    }
	

返回值示例

	无

----------

## 8 指令：PUT /repositories/:repname/:itemname

说明

	【拥有者】更新DataItem

输入参数说明
	
	itemaccesstype  				访问权限[public(默认), private]
	meta							元数据
	sample							样例数据
	comment							详情
    price					        计费计划
    price.units                     购买数量
    price.money                     价格
    price.expire                    有效期(天)
    price.limit                     限购次数【可选】
			
Example Request：

	PUT /repositories/chinamobile/beijingphone HTTP/1.1 
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	{
        "itemaccesstype": "private",
        "meta": "{}",
        "sample": "{}",
		"price":[
					{
                    	"units": "30",
                        "money": 5,
                        "expire":30,
                        "limit":1,
                        "plan_id":"100000000000000000000001"
                    },
                    {
                    	"units": "30",
                        "money": 5,
                        "expire":30,
                        "limit":1,
                        "plan_id":"100000000000000000000002"
                    },
                    {
                        "units": "30",
                        "money": 5,
                        "expire":30,
                        "limit":1,
                    }
				],
        "comment": "对终端使用情况、变化情况进行了全方面的分析。包括分品牌统计市场存量、新增、机型、数量、换机等情况。终端与ARPU、DOU、网龄的映射关系。终端的APP安装情况等。"      
    }
	
返回值示例

	无

----------

## 9 指令：DELETE /repositories/:repname/:itemname

说明

	【拥有者】删除DataItem

输入参数说明

	无

Example Request：

	DELETE /repositories/chinamobile/beijingphone HTTP/1.1 
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	
返回值示例
	
	无
----------

## 10 GET /repositories/:repname/:itemname/:tag

说明

	【任意】查询DataItem下的Tag详情，注意Tag需要按照UTF8编码后传递

输入参数说明：

	无

Example Request：

	GET /repositories/chinamobile/beijingphone/TAG000 HTTP/1.1

返回参数说明：
	
	comment  daemon提交tag相关详情
	optime	 daemon提交tag时间
	
返回值示例
	
	{
	    "comment": "50M",
	    "optime": "2015-08-03 00:00:00|6天以前"
	}

## 11 POST /repositories/:repname/:itemname/:tag

说明

	【拥有者】发布DataItem下的Tag详情，注意Tag需要按照UTF8编码后传递
	 tag字符串最长为20个字符

输入参数说明：

	comment  tag相关详情

Example Request：

	POST /repositories/chinamobile/beijingphone/000 HTTP/1.1
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb 
	{
		"comment":"2001MB"
	}
	
Example Response：
	
	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json
	
	
返回数据说明：

	msg：可选，具体出错信息描述

## 12 PUT /repositories/:repname/:itemname/:tag

说明

	【拥有者】更新tag

输入参数说明：

	comment  tag相关详情

Example Request：

	PUT /repositories/chinamobile/beijingphone/000 HTTP/1.1
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb 
	{
		"comment":"2001MB"
	}
	
Example Response：
	
	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json
	
	
返回数据说明：

	msg：可选，具体出错信息描述

## 13 DELETE /repositories/:repname/:itemname/:tag

说明

	【拥有者】删除DataItem下的Tag详情，注意Tag需要按照UTF8编码后传递

输入参数说明：

	无

Example Request：

	DELETE /repositories/chinamobile/beijingphone/TAG000 HTTP/1.1
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb 
	
Example Response：
	
	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json

## 14 [GET] /repositories/:repname/:itemname/subpermission

说明

	[Subscription] 查询某用户是否具有订阅权限

输入参数说明：

	无

Example Request：

	GET /repositories/chinamobile/beijingphone/subpermission HTTP/1.1
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb 
	
Example Response：
	
	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json
	{
		"create_user": "panxy3",
		"itemaccesstype": "private",
		"optime": "2015-08-0100: 00: 00",
		"meta": {},
		"sample": {},
		"comment": "对终端使用情况、变化情况进行了全方面的分析。包括分品牌统计市场存量、新增、机型、数量、换机等情况。终端与ARPU、DOU、网龄的映射关系。终端的APP安装情况等。",
		"tags":10
		"label": {
		    "sys": {
		        "supply_style": "api"
		     },
		     "opt": {},
		     "owner": {},
		     "other": {}
		}	
	}

	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json

## 14 [GET] /repositories/deleted

说明

   【拥有者】查询删除的rep和item

输入参数说明：

	无

Example Request：

	GET /repositories/deleted HTTP/1.1
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb 
	
Example Response：
	
	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json
	[
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "test",
                "optime": "2016-01-12 11:24:59.902328349 +0800 CST",
                "items": 0,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "0001-01-01T00:00:00Z",
                "Cooperate": null
            },
            "dataitems": [
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:45.266051955 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:39.213718944 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:02:28.28178537 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:02:09.142122461 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:01:41.718104405 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                }
            ]
        },
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "中国移动北京终端详情",
                "optime": "2016-01-13 19:25:14.723231253 +0800 CST",
                "items": 0,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "0001-01-01T00:00:00Z",
                "Cooperate": null
            },
            "dataitems": [
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:45.266051955 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:39.213718944 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:02:28.28178537 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:02:09.142122461 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:01:41.718104405 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                }
            ]
        },
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "中国移动北京终端详情",
                "optime": "2016-01-13 19:26:45.266051955 +0800 CST",
                "items": 2,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "0001-01-01T00:00:00Z",
                "Cooperate": null
            },
            "dataitems": [
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:45.266051955 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:26:39.213718944 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "publ         ic",
                    "optime": "2016-01-13 19:02:28.28178537 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:02:09.142122461 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                },
                {
                    "create_user": "panxy3@asiainfo.com",
                    "itemaccesstype": "public",
                    "optime": "2016-01-13 19:01:41.718104405 +0800 CST",
                    "Meta": "",
                    "Sample": "",
                    "comment": "中国移动北京终端详情",
                    "tags": 0,
                    "label": {
                        "opt": {},
                        "other": {},
                        "owner": {},
                        "sys": {
                            "supply_style": "api"
                        }
                    }
                }
            ]
        },
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "update中国移动北京终端详情",
                "optime": "2016-01-15 16:07:03.896363198 +0800 CST",
                "items": 0,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "0001-01-01T00:00:00Z",
                "Cooperate": null
            },
            "dataitems": []
        },
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "update中国移动北京终端详情",
                "optime": "2016-01-15 16:02:18.388574131 +0800 CST",
                "items": 0,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "2016-01-15T16:09:46.207+08:00",
                "Cooperate": null
            },
            "dataitems": []
        },
        {
            "repository": {
                "create_user": "panxy3@asiainfo.com",
                "repaccesstype": "public",
                "comment": "update中国移动北京终端详情",
                "optime": "2016-01-15 16:11:13.263683341 +0800 CST",
                "items": 0,
                "cooperateitems": 0,
                "label": {
                    "opt": {},
                    "other": {},
                    "owner": {},
                    "sys": {}
                },
                "St": "2016-01-15T16:11:42.4+08:00",
                "Cooperate": null
            },
            "dataitems": []
        }
    ]

	HTTP/1.1 200 
	Vary: Accept 
	Content-Type: application/json