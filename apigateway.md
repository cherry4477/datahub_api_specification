# API 列表

- [PUT] /apiinfo/:reponame/:itemname
- [GET] /apiinfo/:reponame/:itemname

----------

##1 指令：PUT /apiinfo/:reponame/:itemname

说明：
	插入和更新api网关中的item
	
输入参数说明：
	
	url: 请求地址的url
	method: 请求类型(1:GET 2:POST)
	verify: 是否需要认证
	key: 认证的key
	params: 请求参数(name:参数名称 must:是否是必要参数(0:不是 1:是) type:参数类型)
Example Request：
	
	PUT /apiinfo/Finance/TkByTheme HTTP/1.1 
	Accept: application/json
	Content-Type: application/json
	{
  		"url":"way.jd.com/datayes/getTkByTheme",
  		"method":1,
  		"verify":1,
  		"key":"appkey=537e2e5f0c948d4a6cd88c097918feee",
  		"params":[{"name":"themeID","must":0,"type":"string"},{"name":"themeName","must":0,"type":"string"},			{"name":"beginDate","must":0,"type":"string"},{"name":"endDate","must":0,"type":"string"},{"name":"isNew","must":0,"type":"int"},{"name":"field","must":0,"type":"string"}]
	}
返回数据说明：

    

返回数据示例
   
	{
	    "code": 200,
	    "msg": "OK",
	}



##1 指令：GET /apiinfo/:reponame/:itemname

说明：
	调用api
	
输入参数说明：
	
	authuser: 请求用户名(不加前缀)
	apitoken: 请求用户的token
	
Example Request：
	
	GET /apiinfo/Finance/TkByTheme?authuser=aaaaa@aaaa.com&apitoken=ddafsgfadgdfgdsg HTTP/1.1 
	Accept: application/json
	Content-Type: application/json
	
返回数据说明：

    	

返回数据示例
   
	{
	    "code": 200,
	    "msg": "OK",
	    "data":
	}
	
