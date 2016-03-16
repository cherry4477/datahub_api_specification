# Transactions

## Errors

	ErrorCodeUnkown                = 5000
	ErrorCodeJsonBuilding          = 5001
	ErrorCodeUrlNotSupported       = 5002
	ErrorCodeDbNotInitlized        = 5003
	ErrorCodeAuthFailed            = 5004
	ErrorCodePermissionDenied      = 5005
	ErrorCodeInvalidParameters     = 5006
	ErrorCodeGetDataItem           = 5007
	ErrorCodeCreateSubscription    = 5008
	ErrorCodeGetSubscription       = 5009
	ErrorCodeCancelSubscription    = 5010
	ErrorCodeQuerySubscription     = 5011
	ErrorCodeSubscriptionNotFound  = 5012
	ErrorCodeCreateTransaction     = 5013
	ErrorCodeGetTransaction        = 5014
	ErrorCodeQueryTransaction      = 5015
	ErrorCodeGetStatistics         = 5016
	ErrorCodeParseJsonFailed       = 5017
	ErrorCodeFailedToConnectRemote = 5018
	ErrorCodeNotOkRemoteResponse   = 5019
	ErrorCodeInvalidRemoteResponse = 5020
	ErrorCodeTokenNotFound         = 5021
	ErrorCodeTokenExpird           = 5022
	ErrorCodeInvalidEntryPoints    = 5023

## APIs

### POST /transaction/:repname/:itemname/:tag (50)

说明

	【需求者】返回该请求的access token
	
	注意：tag需要UTF8编码，注意后面没有s

输入参数说明：
	
	无

输入样例：

	POST /transaction/repo1/item123/tag2 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：

	"accesstoken":"a1a2a3a4a5a6a7a8"
	"remainingtime":"72h3m0.5s"
	"entrypoint":"http://www.exaple.com:5678"

### GET /transaction/:repname/:itemname/:tag?cypt_accesstoken={cypt_accesstoken}&username={username} (51)

说明

	【拥有者】校验该cypt_accesstoken的有效性，此cypt_accesstoken被需求者的私有证书加密（但目前并未实现加密）

输入参数说明：
	
	cypt_accesstoken: 需求者的access token 
	username: 需求者用户名

样例输入：

	GET /transaction/repo1/item123/tag2?cypt_accesstoken=a1b1c1d2abcdadas&username=zhang3@example.com HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

样例输出：
        
	"valid":true
	"remainingtime":"33h3m0.5s"

返回数据说明：

	valid: 输入access token是否有效
	remainingtime: 剩余有效期。可能的结果："33h3m0.5s", "3m2s", "0"

### GET /transaction_stat/:repname/:itemname (52)

说明

	【任意】返回该DataItem的pull量

输入参数说明：
	
	无

输入样例（非匿名用户需加Auth header）：

	POST /transaction_stat/repo1/item123 HTTP/1.1 
	Accept: application/json

输出样例：

	"numpulls":567，
	“nummypulls”:12

返回数据说明：

	numpulls: 所有人总共产的pull量
	nummypulls: 当前用户产生的pull量 (匿名请求无此项)
	
### GET /transaction_stat/:repname (53)

说明

	【任意】返回该repositories的pull量

输入参数说明：
	
	无

输入样例（非匿名用户需加Auth header）：

	POST /transaction_stat/repo1 HTTP/1.1 
	Accept: application/json

输入样例：

	"numpulls":567，
	“nummypulls”:12

返回数据说明：

	numpulls: 所有人总共产的pull量
	nummypulls: 当前用户产生的pull量 (匿名请求无此项)

### GET /transaction_stat/:repname/:itemname/:tag (54)

说明

	【任意】返回该tag的pull量

输入参数说明：
	
	无 

输入样例（非匿名用户需加Auth header）：

	POST /transaction_stat/repo1/item123/tag3 HTTP/1.1 
	Accept: application/json

输出样例：

	"numpulls":567，
	“nummypulls”:12

返回数据说明：

	numpulls: 所有人总共产的pull量
	nummypulls: 当前用户产生的pull量 (匿名请求无此项)

### GET /transactions/pull?groupbydate=[0|1]&page={page}&size={size}  (55)

说明

	【需求者】返回该用户所有的pull的信息

输入参数说明：
	
	groupbydate: (可选，默认为0) 是否按日期分组
	page: (可选) 第几页，最小值为1
	size: (可选) 每页最多返回多少条数据

输入样例：

	GET /transactions/pull?groupbydate=1 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例(groupbydate=1)：

	{
		"total": 100,
		"results": [
			{
				"date":"2015-11-18",
				"pulls":[
					{
						"sellername":"li4",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-18T11:48:07Z",
						"subscriptionid": 12345,
						"supply_style": "batch"
					},
					{
						"sellername":"li4",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-18T11:29:38Z",
						"subscriptionid": 567567,
						"supply_style": "flow"
					},
					{
						"sellername":"li4",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-18T11:29:31Z",
						"subscriptionid": 99999,
						"supply_style": "batch"
					}
				]
			},
			{
				"date":"2015-11-10",
				"pulls":[
					{
						"sellername":"li4",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-10T10:10:10Z",
						"subscriptionid": 98765,
						"supply_style": "flow"
					}
				]
			}
		]
	}
	
	输出样例(groupbydate=0)：
	        
	{
		"total": 100,
		"results": [
			{
				"sellername": "Li4",
				"repname": "repo1231",
				"itemname": "item9883",
				"tag": "tag8W",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 12345,
				"supply_style": "batch"
			},
			{
				"sellername": "Li4",
				"repname": "repo121",
				"itemname": "item989",
				"tag": "tag09",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 98765,
				"supply_style": "flow"
			}
		]
	}

返回数据说明：

	sellername: 提供者用户名
	repname: repository name
	itemname: data item name
	tag: tag名
	pulltime: pull time, RFC3339 format

### GET /transactions/pull/:repname/:itemname?groupbydate=[0|1]&page={page}&size={size}  (56)

说明

	【需求者】返回该用户pull的该DataItem上的tags

输入参数说明：
	
	groupbydate: (可选，默认为0) 是否按日期分组
	page: (可选) 第几页，最小值为1
	size: (可选) 每页最多返回多少条数据

输入样例：

	GET /transactions/pull/repo03/item56 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"tag": "tag8W",
				"pulltime": "2015-11-09T15:04:05Z07:00",
				"subscriptionid": 12345,
				"supply_style": "batch"
			},
			{
				"tag": "tag09",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 98765,
				"supply_style": "flow"
			}
		]
	}

返回数据说明：

	tag: tag名
	pulltime: pull time, RFC3339 format

### GET /transactions/push?groupbydate=[0|1]&page={page}&size={size}  (57)

说明

	【拥有者】返回该用户所有的被人家Pull的信息

输入参数说明：
	
	groupbydate: (可选，默认为0) 是否按日期分组
	page: (可选) 第几页，最小值为1
	size: (可选) 每页最多返回多少条数据

输入样例：

	GET /transactions/push?groupbydate=1 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例(groupbydate=1)：

	{
		"total": 100,
		"results": [
			{
				"date":"2015-11-18",
				"pulls":[
					{
						"buyername":"zhang3",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-18T11:48:07Z",
						"subscriptionid": 678,
						"supply_style": "flow"
					},
					{
						"buyername":"John",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-18T11:29:38Z",
						"subscriptionid": 32323,
						"supply_style": "batch"
					}
				]
			},
			{
				"date":"2015-11-10",
				"pulls":[
					{
						"buyername":"zhang3",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-10T10:10:10Z",
						"subscriptionid": 98765,
						"supply_style": "flow"
					},
					{
						"buyername":"zhang3",
						"repname":"repo001",
						"itemname":"item002",
						"tag":"tag008",
						"pulltime":"2015-11-10T10:10:10Z",
						"subscriptionid": 123456,
						"supply_style": "batch"
					},
				]
			}
		]
	}

输出样例(groupbydate=0)：
        
	{
		"total": 100,
		"results": [
			{
				"buyername": "Li4",
				"repname": "repo1231",
				"itemname": "item9883",
				"tag": "tag8W",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 98765,
				"supply_style": "flow"
			},
			{
				"buyername": "Smith",
				"repname": "repo121",
				"itemname": "item989",
				"tag": "tag09",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 123456,
				"supply_style": "batch"
			}
		]
	}

返回数据说明：

	buyername: 需求者用户名
	repname: repository name
	itemname: data item name
	tag: tag名
	pulltime: pull time, RFC3339 format

### GET /transactions/push/:repname/:itemname?groupbydate=[0|1]&page={page}&size={size}  (58)

说明

	【需求者】返回该用户用有的该DataItem被人家Pull的信息。

输入参数说明：
	
	groupbydate: (可选，默认为0) 是否按日期分组
	page: (可选) 第几页，最小值为1
	size: (可选) 每页最多返回多少条数据

输入样例：

	GET /transactions/push/repo03/item56?groupbydate=1 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例(groupbydate=1)：

	{
		"total": 100,
		"results": [
			{
				"date":"2015-11-10",
				"pulls"[
					{
						"buyername": "Li4",
						"tag": "tag8W",
						"pulltime": "2015-11-10T15:04:05Z07:00",
						"subscriptionid": 123456,
						"supply_style": "batch"
					},
					{
						"buyername": "Zhang3",
						"tag": "tag09",
						"pulltime": "2015-11-10T10:09:05Z07:00",
						"subscriptionid": 789,
						"supply_style": "flow"
					}
				]
			},
			{
				"date":"2015-11-03",
				"pulls"[
					{
						"buyername": "Zhang3",
						"tag": "tag09",
						"pulltime": "2015-11-03T15:06:05Z07:00",
						"subscriptionid": 789,
						"supply_style": "flow"
					}
				]
			}
		]
	}

输出样例(groupbydate=0)：
        
	{
		"total": 100,
		"results": [
			{
				"buyername": "Li4",
				"tag": "tag8W",
				"pulltime": "2015-11-10T15:04:05Z07:00",
				"subscriptionid": 123456,
				"supply_style": "batch"
			},
			{
				"buyername": "Zhang3",
				"tag": "tag09",
				"pulltime": "2015-11-10T15:09:05Z07:00",
				"subscriptionid": 789,
				"supply_style": "flow"
			},
			{
				"buyername": "Zhang3",
				"tag": "tag09",
				"pulltime": "2015-11-03T15:06:05Z07:00",
				"subscriptionid": 789,
				"supply_style": "flow"
			}
		]
	}

返回数据说明：

	buyername: 需求者用户名
	tag: tag名
	pulltime: pull time, RFC3339 format
	
