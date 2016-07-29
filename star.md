# Star APIs

### (A0) GET /star/:repname/:itemname

说明

	【用户】查询是否star某个DataItem

输入参数说明：
	
	无

输入样例：

	GET /star/repo1/item123 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：

	"starred":true

### (A1) PUT /star/:repname/:itemname?star=[0|1]

说明

	【用户】更改对一个DataItem的star状态 

输入参数说明：
	
	star: 0表示unstar, 1表示star

输入样例：

	PUT /star/repo1/item123?star=1 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	"starred":true

### (A2) GET /star_stat/:repname/:itemname

说明

	【任意】返回该DataItem的star量

输入参数说明：
	
	无

输入样例：

	GET /star_stat/repo1/item123 HTTP/1.1 
	Accept: application/json

输出样例：

	"numstars":567

### (A3) GET /star_stat/:repname

说明

	【任意】返回该repository的star量

输入参数说明：
	
	无

输入样例：

	GET /star_stat/repo1 HTTP/1.1 
	Accept: application/json

输出样例：

	"numstars":567

### (A4) GET /stars/pull?page={page}&size={size}&sortorder={asc|desc}

说明

	【需求者】查询所有stars。

输入参数说明：
	
	page: (可选, 默认为1) 第几页，最小值为1。
	size: (可选, 默认为30) 每页最多返回多少条数据, 最小值为1, 最大值为100。
	sortorder: (可选, asc | desc, 默认为desc) 时间排序顺序，asc（升序），desc(降序)。

输入样例：

	GET /stars/pull HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"repname":"NBA",
				"itemname":"bear",
				"optime":"2016-01-10T10:51:11Z"
			},
			{
				"repname":"CBA",
				"itemname":"triger",
				"optime":"2016-01-22T15:04:05Z"
			}
		]
	}

返回数据说明：

	repname: repository name
	itemname: data item name
	optime: 操作时间
	

### (A5) GET /stars/pull/:repname?page={page}&size={size}&sortorder={asc|desc}


说明

	【需求者】查询在某个respository中的所有stars

输入参数说明：
	
	page: (可选, 默认为1) 第几页，最小值为1。
	size: (可选, 默认为30) 每页最多返回多少条数据, 最小值为1, 最大值为100。
	sortorder: (可选, asc | desc, 默认为desc) 时间排序顺序，asc（升序），desc(降序)。

输入样例：

	GET /stars/pull/repo001 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"itemname":"bear",
				"optime":"2016-01-10T10:51:11Z"
			},
			{
				"itemname":"triger",
				"optime":"2015-11-01T15:04:05Z"
			}
		]
	}

### (A6) GET /stars/push?page={page}&size={size}&sortorder={asc|desc}

说明

	【提供者】查询自己的所有dataitem上的所有stars

输入参数说明：
	
	page: (可选, 默认为1) 第几页，最小值为1。
	size: (可选, 默认为30) 每页最多返回多少条数据, 最小值为1, 最大值为100。
	sortorder: (可选, asc | desc, 默认为desc) 时间排序顺序，asc（升序），desc(降序)。

输入样例：

	GET /stars/push HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"username":"zhang3@example.com",
				"repname":"NBA",
				"itemname":"bear",
				"optime":"2016-01-10T10:51:11Z"
			},
			{
				"username":"li4@example.com",
				"repname":"CBA",
				"itemname":"triger",
				"optime":"2016-01-22T15:04:05Z"
			}
		]
	}

返回数据说明：

	username: star者

### (A7) GET /stars/push/:repname?page={page}&size={size}&sortorder={asc|desc}

说明

	【提供者】查询自己的某个repository内的的所有star

输入参数说明：
	
	page: (可选, 默认为1) 第几页，最小值为1。
	size: (可选, 默认为30) 每页最多返回多少条数据, 最小值为1, 最大值为100。
	sortorder: (可选, asc | desc, 默认为desc) 时间排序顺序，asc（升序），desc(降序)。

输入样例：

	GET /stars/push/repo001 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"username":"zhang3@example.com",
				"itemname":"item002",
				"optime":"2015-11-01T15:04:05Z"
			},
			{
				"username":"li4@example.com",
				"itemname":"item119",
				"optime":"2016-01-04T15:04:05Z"
			}
		]
	}

### (A8) GET /stars/push/:repname/:itemname?page={page}&size={size}&sortorder={asc|desc}

说明

	【提供者】查询自己的某个dataitem上的所有star

输入参数说明：
	
	page: (可选, 默认为1) 第几页，最小值为1。
	size: (可选, 默认为30) 每页最多返回多少条数据, 最小值为1, 最大值为100。
	sortorder: (可选, asc | desc, 默认为desc) 时间排序顺序，asc（升序），desc(降序)。

输入样例：

	GET /stars/push/repo001/item002 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：
        
	{
		"total": 100,
		"results": [
			{
				"username":"zhang3@example.com",
				"optime":"2015-11-01T15:04:05Z"
			},
			{
				"username":"li4@example.com",
				"optime":"2016-01-04T15:04:05Z"
			}
		]
	}
