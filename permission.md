# API 列表
	
	
- [GET] [PUT] /permission/:repname

- [GET] [DELETE] [PUT] /permission/:repname/:itemname

- [DELETE] /permission/:repname/whitelist/:username

- [DELETE] /permission/:repname/cooperator/:username

	
----------

##  1 指令：GET /permission/:repname

说明

	【rep拥有者】查询自己rep白名单的username列表，及相应的读写权限

输入参数说明：
	
	username :                      查询某个用户是否在白名[如果是,返回用户名.如果否,返回空]
	cooperator :                    条件查询所有协作者(具有写权限)
    page (分页页数) : 				1 - N，  默认=1（page=1可以不传）
    size（页面大小）: 				1 - N，  默认=6 (-1 返回全部)
    
Example Request：

	GET /permission/rep?username=user1&page=2 HTTP/1.1 
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

Example Response：
	
	    permissions:[
		                {"username":"user1", "opt_permission": 1},	
		                {"username":"user2", "opt_permission": 1}
	               ],
	    total:50             

返回数据说明：

	permissions.username					rep白名单用户名
	permissions.opt_permission				rep白名单用户所具有的权限[0:读,1:写]
	total                                   total总数

----------

## 2 指令：PUT /permission/:repname

说明：
	
	【rep拥有者】将某用户（非自己）加入或更新rep白名单中

输入参数说明：

	username 		被加入白名单的用户名
	opt_permission	默认不传（只有读权限），传1具有写
	
Example Request：

	PUT /permission/rep00001 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	[
		{"username":"user1","opt_permission":1}
	]

返回数据说明：
	
	无
		
## 3 指令：DELETE /permission/:repname/whitelist/:username

说明：
	
	【rep拥有者】将某些用户（非自己）从rep白名单中删除

输入参数说明：

	delall(可选)          是否删除所有白名单用户[delall=1 删除所有]
   
Example Request：

	DELETE /permission/rep001/whitelist/peter HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	
	DELETE /permission/rep001/whitelist/peter?delall=1 HTTP/1.1
    Accept: application/json
    Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb


## 4 指令： DELETE /permission/:repname/cooperator/:username

说明：

	【rep拥有者】将某些用户（非自己）从rep协作者中删除

输入参数说明：

	delall(可选)          是否删除所有白名单用户[delall=1 删除所有]

Example Request：

	DELETE /permission/rep001/cooperator/peter HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

	DELETE /permission/rep001/cooperator/peter?delall=1 HTTP/1.1
    Accept: application/json
    Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb


## 4 指令：GET /permission/:repname/:itemname

说明

	【item拥有者】查询自己item白名单中username列表

输入参数说明：
	
	username 查询某个用户是否在白名[如果是,返回用户名.如果否,返回空]
    page (分页页数) : 				1 - N，  默认=1（page=1可以不传）
    size（页面大小）: 				1 - N，  默认=6 (-1 返回全部)

Example Request：

	GET /permission/repname1/itemname1?page=4 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

返回数据说明：
    
	permissions:[
    		        {"username":"user1"},	
    		        {"username":"user2"},
    		        {"username":"user3"},	
                    {"username":"user4"}
    	        ],
    total:50             
	
----------

## 5 指令：PUT /permission/:repname/:itemname
	
说明：
	
	【item拥有者】将某用户（非自己）加入item白名单中

输入参数说明：

	username 		被加入白名单的用户名
	
Example Request：

	POST /permission/repname1/itemname1 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	[
	    {"username":"user1"}
	]

返回数据说明：
	
	无

## 6 指令：DELETE /permission/:repname/:itemname
	
说明：
	
	【item拥有者】将某些用户（非自己）从item白名单中移除

输入参数说明：

	username 		被从白名单删除的用户名
	delall          是否删除所有白名单用户[delall=1 删除所有]
   
Example Request：

	DELETE /permission/repname1/itemname1?username=abc HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
    
    DELETE /permission/repname1/itemname1?delall=1 HTTP/1.1 
    Accept: application/json
    Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb